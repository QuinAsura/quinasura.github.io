---
layout: post
title: Localization, Obstacle avoidance and Control [Part-1]
categories: articles
tags: [localization,contours,open-cv]
comments: true
excerpt: "Probing for the bot."
share: true
---

Robotics has it's existence at the heart of variety of fields. So instead of taking up a view of robotics as a demanding master that requires the best of all its fields we can consider its potential of delivering theory intensive control theory, artificial intelligence and computer vision in an informal manner. Nevertheless the learning and insight it can provide while undertaking robotics projects is one of the more salient features that we all miss.

#Demo
This is where the working video post is displayed.

##Tools needed
* Webcam (list of webcams)
* opencv 2.4 or later (installation and settin up in ubuntu)
* access to a two wheeled mobile robot(differential steering system) equipped with a microcontroller (Arduino or MSP430 in our case).
* Patience and the drive to bring your creation into existence.
* a little hacking in c++

In this series of post we take up the task of localization, control and planning of a differential steering system. The differential steering robot is easily accessible and cost of construction is very small. You can either take an off the shelf robot for the task but I sincerely hope that you guys choose the latter. 

#What is localization?
In robotics, localization is fundemental task. Especially in wheeled robots it is performed using odometers or using a camera to perform visual odometry. However, in our toy problem the localization task involves to find the coordinates of the bot in the span of the webcam mounted on the ceiling. The performance constraints that we place require us to use opencv with c++. Again identification of bot can be performed in a variety of ways. Since we consider this an introduction of sorts we simple prefer to cover our bot with a hard color for simplying the task of detection. It is considered a toy problem since we cannot have overhead camera static camera outdoors. Nowadays, quadcopter can substitute the static camera.

#Detecting Color using cv contour
The problem of using it varies from intensity RGB. However if you consider chromaticity. Explain chromaticity. Youtube link for basic countour detection. Erosion and dilation are complete subjects in themselves.

#Localization implementation class with explanation
This necessary to identify the pose of the robot.

{% highlight c++ %}

void GetXY(Mat frame)
        {
            Mat imgHSV;
            namedWindow("MyVideo",CV_WINDOW_AUTOSIZE);
            cvtColor(frame, imgHSV, COLOR_BGR2HSV); //Convert the captured frame from BGR to HSV
            Mat imgThresholded;
            Mat greenthresh;
            inRange(imgHSV, Scalar(yLowH, yLowS, yLowV), Scalar(yHighH, yHighS, yHighV), imgThresholded); //Threshold the image
            inRange(imgHSV, Scalar(gLowH, gLowS, gLowV), Scalar(gHighH, gHighS, gHighV), greenthresh);
            //morphological opening (removes small objects from the foreground)
            erode(greenthresh, greenthresh, getStructuringElement(MORPH_ELLIPSE, Size(5, 5)) );
            erode(imgThresholded, imgThresholded, getStructuringElement(MORPH_ELLIPSE, Size(5, 5)) );
            dilate( greenthresh, greenthresh, getStructuringElement(MORPH_ELLIPSE, Size(5, 5)) ); 
            dilate( imgThresholded, imgThresholded, getStructuringElement(MORPH_ELLIPSE, Size(5, 5)) ); 
            //morphological closing (removes small holes from the foreground)
            dilate( imgThresholded, imgThresholded, getStructuringElement(MORPH_ELLIPSE, Size(5, 5)) ); 
            dilate( greenthresh, greenthresh, getStructuringElement(MORPH_ELLIPSE, Size(5, 5)) ); 
            erode(imgThresholded, imgThresholded, getStructuringElement(MORPH_ELLIPSE, Size(5, 5)) );
            erode(greenthresh, greenthresh, getStructuringElement(MORPH_ELLIPSE, Size(5, 5)) );

            Mat canny_output;
            vector<vector<Point> > contours;
            vector<vector<Point> > gcontours;
            vector<Vec4i> hierarchy;
            vector<Vec4i> ghierarchy;

            findContours(imgThresholded,contours, hierarchy, CV_RETR_TREE, CV_CHAIN_APPROX_SIMPLE, Point(0, 0) );
            findContours(greenthresh,gcontours, ghierarchy, CV_RETR_TREE, CV_CHAIN_APPROX_SIMPLE, Point(0, 0) );
            //cout<<contours.size()+gcontours.size()<<endl;
            vector<Moments> mu(contours.size() );
            vector<Moments> gmu(gcontours.size() );

            for( int i = 0; i < contours.size(); i++ )
            { mu[i] = moments( contours[i], false ); }

            for( int i = 0; i < gcontours.size(); i++ )
            { gmu[i] = moments( gcontours[i], false ); }
            int maxArea =0;
            int bestcnt =0;
            for(int i = 0; i < contours.size(); i++)
            { 
                if (mu[i].m00 > maxArea)
                {
                    maxArea = mu[i].m00;
                    bestcnt = i;
                }
            }
            int gmaxArea = 0;
            int gbestcnt = 0;
            for(int i = 0; i < gcontours.size(); i++)
            { 
                if (gmu[i].m00 > gmaxArea)
                {
                    maxArea = gmu[i].m00;
                    gbestcnt = i;
                }
            }

            if (!((gcontours.size()==0) || (contours.size()==0)))
            {
                Yc = Point(mu[bestcnt].m10/mu[bestcnt].m00 , mu[bestcnt].m01/mu[bestcnt].m00 );
                Gc = Point(gmu[gbestcnt].m10/gmu[gbestcnt].m00 , gmu[gbestcnt].m01/gmu[gbestcnt].m00 );
            }
        }
}
{% endhighlight %}

{% highlight c++ %}
double GoToGoal(Point goal){
            Point oriented_dir = Gc-Yc;
            Point goal_dir = Gc - goal;
            double dot = oriented_dir.x*goal_dir.x + oriented_dir.y*goal_dir.y;;
            double det = oriented_dir.x*goal_dir.y - oriented_dir.y*goal_dir.x;
            //cout<<((atan2(det,dot))*180/PI)<<endl;
            return (atan2(det,dot));
        }
}
{% endhighlight %}

