---
layout: post
title: Localization, Obstacle avoidance and Control [Part-3]
author: sibi_sank
comments: true
share: true
excerpt: "Incorporating a little artificial intelligence"
tags: [Artificial-Intelligence, A_star,A*,Path-planning,trajectory-tracking]
---

#Path planning
* Need for path planning
* Search methods
* A_star is the king of search and heuristic
* Links to videos showing the thing in action edx AI course by UC-Berkeley

#Kinematically feasible paths
* code
* weights assigned to nodes

#Putting the entire thing together
*Future plans we plan to make an entire swarm
*To achieve this we started using CUDA the gpu which allows for parallel computation
*Perform the arctan calculation in the robots itself


{% highlight c++ %}
struct SearchCell
{
    public:
        int m_xcoord, m_zcoord;
        int m_id;
        SearchCell * parent;
        float G;
        float H;

        SearchCell()
        {
            parent =0;
        }

        SearchCell(int x, int z, SearchCell * _parent)//parent =0
        {
            m_xcoord = x;
            m_zcoord = z;
            parent = _parent;
            m_id = z*WORLD_SIZE + x;
            G=0;
            H=0;
        }

        float GetF()
        {
            return G+H;
        }

        float ManHattanDistance(SearchCell * nodeEnd)
        {
            float x = (float)(fabs((float)(this->m_xcoord- nodeEnd->m_xcoord)));
            float z = (float)(fabs((float)(this->m_zcoord- nodeEnd->m_zcoord)));
            return x+z;
        }
};
{% endhighlight %}


{% highlight c++ %}
class PathFinding
{
    public:
        bool m_intializedStartGoal;
        bool m_foundGoal;
        SearchCell *m_startCell;
        SearchCell *m_goalCell;
        vector<SearchCell*> m_openList;
        vector<SearchCell*> m_visitedList;
        vector<Point3f> m_pathToGoal;
        Mat SearchSpace;


        PathFinding(Mat graph)
        {
            SearchSpace = graph;
            m_intializedStartGoal = false;
            m_foundGoal = false;
        }

        void FindPath(Point3f currentPos, Point3f targetPos)
        {
            if(!m_intializedStartGoal)
            {
                m_openList.erase(m_openList.begin(),m_openList.end());
                m_visitedList.erase(m_visitedList.begin(),m_visitedList.end());
                m_pathToGoal.erase(m_pathToGoal.begin(),m_pathToGoal.end());

                SearchCell start;
                start.m_xcoord = currentPos.x;
                start.m_zcoord = currentPos.z;
                SearchCell goal;
                goal.m_xcoord = targetPos.x;
                goal.m_zcoord = targetPos.z;
                SetStartAndGoal(start,goal);
                m_intializedStartGoal = true;
            }

            while((!m_foundGoal) && !(m_openList.empty()))
            {
                ContinuePath();
            }

            if(m_openList.empty())
            {
                cout<<">>Found No Path to Goal: Terminating"<<endl;
                cout<<">>Reset the Box Size"<<endl;
            }

        }

        void SetStartAndGoal(SearchCell start,SearchCell goal)
        {
            m_startCell = new SearchCell(start.m_xcoord,start.m_zcoord,NULL);
            m_goalCell = new SearchCell(goal.m_xcoord,goal.m_zcoord,NULL);//Made a change from &goal
            m_startCell->G=0;
            m_startCell->H= m_startCell->ManHattanDistance(m_goalCell);
            m_startCell->parent = 0;
            m_openList.push_back(m_startCell);
        }

        void PathOpened(int x,int z,float newCost,SearchCell* parent)
        {

            if(CellBCheck(x,z))
            {
                return;
            }

            int id = z * WORLD_SIZE + x;
            for(int i=0;i< m_visitedList.size();i++)
            {
                if(id == m_visitedList[i]->m_id)
                {
                    return;
                }
            }
            SearchCell * newChild = new SearchCell(x,z,parent);
            newChild->G = newCost;
            newChild->H = newChild->ManHattanDistance(m_goalCell);//replaced parent with newchild
            m_openList.push_back(newChild);
        }

        SearchCell *GetNextCell()
        {
            float bestF = 9999999.0f;
            int cellIndex = -1;
            SearchCell * nextCell = NULL;
            for(int i=0; i < m_openList.size();i++)
            {
                if(m_openList[i]->GetF()< bestF)
                {
                    bestF = m_openList[i]->GetF();
                    cellIndex = i;
                }
            }

            if(cellIndex>=0)
            {
                nextCell = m_openList[cellIndex];
                m_visitedList.push_back(nextCell);
                m_openList.erase(m_openList.begin()+ cellIndex);
            }
            return nextCell;
        }

        bool CellBCheck(int x,int z)
        {
            if((x<SearchSpace.size().width && x>=0) && (z<SearchSpace.size().height && z>=0))
            {
                if(SearchSpace.at<double>(z,x)==0)
                {
                    return false;
                }
                else
                {
                    return true;
                }
            }
            else
            {
                return true;
            }

        }

        vector<Point3f> RetPlan()
        {
            return m_pathToGoal;
        }

        void ContinuePath()
        {

            SearchCell * currentCell = GetNextCell();
            if(currentCell->m_id == m_goalCell->m_id)
            {
                SearchCell* getPath;
                for(getPath = currentCell; getPath!= NULL; getPath= getPath->parent)
                {
                    m_pathToGoal.push_back(Point3f(getPath->m_xcoord,0,getPath->m_zcoord));
                }
                m_foundGoal = true;
                return;
            }
            else
            {
                //rightCell
                PathOpened(currentCell->m_xcoord+1,currentCell->m_zcoord,currentCell->G+1,currentCell);
                //leftCell
                PathOpened(currentCell->m_xcoord-1,currentCell->m_zcoord,currentCell->G+1,currentCell);
                //topCell
                PathOpened(currentCell->m_xcoord,currentCell->m_zcoord+1,currentCell->G+1,currentCell);
                //bottomCell
                PathOpened(currentCell->m_xcoord,currentCell->m_zcoord-1,currentCell->G+1,currentCell);
                //diagnaltopLeft
                PathOpened(currentCell->m_xcoord-1,currentCell->m_zcoord+1,currentCell->G+1.414,currentCell);
                //diagnaltopRight
                PathOpened(currentCell->m_xcoord+1,currentCell->m_zcoord+1,currentCell->G+1.414,currentCell);
                //diagnalbottomLeft
                PathOpened(currentCell->m_xcoord-1,currentCell->m_zcoord-1,currentCell->G+1.414,currentCell);
                //diagnalbottomRight
                PathOpened(currentCell->m_xcoord+1,currentCell->m_zcoord-1,currentCell->G+1.414,currentCell);
                for(int i=0; i < m_openList.size();i++)
                {
                    if(currentCell->m_id == m_openList[i]->m_id)
                    {
                        m_openList.erase(m_openList.begin()+i);
                    }
                }

            }
        }
};

{% endhighlight %}
