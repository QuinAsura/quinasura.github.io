---
layout: post
author: sibi_sank
title: Kinematics and Control
tags: [differential-drive,differential-steering,learn-by-making,uni-cycle]
comments: true
excerpt: "Adding a little control theory into the mix."
share: true
---

In the last post(Link to post) we saw a simple way of extracting the coordinates of a differential-steering system. Now let us move on to the cool things you can accomplish using the localization information. Before that we need to construct the bot. For people planning to make the entire thing I have included a PCB schematic file for the electronics part. Once you are exposed to the kinematics, it is possible to design you own bot.s

#Unicycle kinematics
*Kinematic
*Need for unicycle dynamic
*Picture

#Differential kinematics
*Expression of left and right wheel velocity (link to complete derivation)

#Switching between the two
*Link between the two

#Some Geometry
*arctan() between the two vectors

#PID
*Leave the tuning to you
*Explain each part of the equation
*kinematic class
*Arduino code with serial communication

{% highlight c++ %}
class Bot
{
    public:
        double Wradius,Blength,Bvelocity,pwm_lower,pwm_upper,omega_lower,omega_upper,rpm_lower,rpm_upper;
        int Wvelocities[2];
        double Kp,Ki,Kd,error,prev_error,Integ;
        long int time,prev_time,freq;

        Bot(double p,double i,double d)
        {
            Kp=p;Ki=i;Kd=d;error=0;prev_error=0;Integ=0;
            time=0;prev_time=0;freq = getTickFrequency();
            Wradius = 0.037;
            Blength = 0.106;
            Bvelocity = 0.065;
            pwm_lower = 45.0;
            pwm_upper = 240.0;
            omega_lower = 0.0;
            omega_upper = 8.35;

        }

        void Uni2DiffKinematics(double w)
        {
            double sum  = 2*Bvelocity/(Wradius);
            double diff = Blength*w/(Wradius);
            double l_w = (sum+diff)/2;
            double r_w = (sum-diff)/2;
            if ((l_w > omega_upper) || (r_w > omega_upper))
            {
                if (l_w > omega_upper)
                {
                    double temp = l_w - omega_upper;
                    l_w = omega_upper;
                    r_w -= temp;
                    if(r_w < omega_lower)
                    {
                        r_w =omega_lower;
                    }

                }
                else
                {
                    double temp = r_w -omega_upper;
                    r_w = omega_upper;
                    l_w -= temp;
                    if(l_w < omega_lower)
                    {
                        l_w=omega_lower;
                    }
                }
            }
            else if ((l_w < omega_lower) || (r_w < omega_lower))
            {
                if(l_w < omega_lower)
                {
                    double temp = omega_lower - l_w;
                    l_w = omega_lower;
                    r_w += temp;
                    if (r_w > omega_upper)
                    {r_w = omega_upper;}
                }
                else
                {
                    double temp = omega_lower - r_w;
                    r_w = omega_lower;
                    l_w += temp;
                    if(l_w > omega_upper)
                    {
                        l_w = omega_upper;
                    }
                }
            }
            l_w = pwm_lower + (((pwm_upper - pwm_lower)/(omega_upper -omega_lower))*l_w);
            r_w = pwm_lower + (((pwm_upper - pwm_lower)/(omega_upper -omega_lower))*r_w);
            Wvelocities[0]=int(l_w);
            Wvelocities[1]=int(r_w);
            //cout<<"Left_wheel:"<<l_w<<"\t"<<"Right_wheel:"<<r_w<<"\t"<<"PID_Value"<<w<<endl;
        }

        void ResetController()
        {
            prev_time = getTickCount();
            error=0;prev_error=0;Integ=0;
        }

        double PID_Controller(double accel)
        {
            double signal;
            error = accel;
            Integ+=error;
            time = getTickCount();
            double dt = (double(time - prev_time))/freq;
            signal = (error*Kp) + ((error - prev_error)*Kd*dt) + (Integ*Ki/dt);
            prev_error = error;
            prev_time = time;
            return signal;
        }
};

{% endhighlight %}

{% highlight c++ %}
class SerialComm
{
    public:
        SerialStream node;

        SerialComm()
        {
            //Set up the required baudrate higher the better
            node.SetBaudRate(SerialStreamBuf::BAUD_9600);
            node.SetCharSize(SerialStreamBuf::CHAR_SIZE_8);
            node.Open(PORT);
        }

};
{% endhighlight %}

{% highlight c %}
//The arduino code on the bot is to extract the string it receives from the computer in the form #132#142!
//However if the input & stops the bot example: #149&151! stops the bot 

//LM -left motor
//RM -right motor
//LED -debugging
//E1,E2 -PWM pin

#define E1 9
#define E2 10
#define LM1 4
#define LM2 5
#define RM1 6
#define RM2 7
#define LED 11

// String to hold incoming data
String inputString = "";

// Indicates Whether the string is complete 
boolean stringComplete = false; 

void setup() 
{
  Serial.begin(9600);
  inputString.reserve(3);
  pinMode(E1,OUTPUT);
  pinMode(E2,OUTPUT);
  pinMode(LM1,OUTPUT);
  pinMode(LM2,OUTPUT);
  pinMode(RM1,OUTPUT);
  pinMode(RM2,OUTPUT);
  pinMode(LED,OUTPUT);
  digitalWrite(E1,0);
  digitalWrite(E2,0);
}

void loop() {
  // print the string when a newline arrives:
  if (stringComplete) {
    int a = inputString.substring(1,4).toInt();
    int b = inputString.substring(5,8).toInt();
    Serial.println(inputString);
    
    //Setting up the PWM values
    analogWrite(E1,b);
    analogWrite(E2,a);
    
    //Setting up the direction in the l293d driver
    digitalWrite(LM1,1);
    digitalWrite(LM2,0);
    digitalWrite(RM1,1);
    digitalWrite(RM2,0);
    
    if (inputString.substring(4,5)=="&")
    {
      digitalWrite(LED,1);
      delay(1000);
      //Serial.println(1);
      digitalWrite(LED,0);
    }
    inputString = "";
    stringComplete = false;
  }
}

void serialEvent()
{
  while (Serial.available()) {
    char inChar = (char)Serial.read();
    inputString += inChar;
    if (inChar == '!') {
      stringComplete = true;
    } 
  }
}
{% endhighlight %}
