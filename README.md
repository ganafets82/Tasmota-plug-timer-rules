Tasmota-plug-timer-rules
Tasmota plug Timer Rules and auto off for 2kw Car charger (and other things)


Var1: Always on mode (long button press, duration with SetOption32 in 1/10sec)
Var2: Timer value (adds 1h with every short button press) 

Turn off: Long button press, or power < 100W for 60s
Timer: Short button presses activates Timer and/or adds 60h


SetOption1 1
SetOption32 15
SetOption53 1
SetOption55 1
SetOption56 1
SetOption73 1



Rule1
ON button1#state=3 DO Backlog Power1 2; Var1 1; Var2 0 ENDON
ON Button1#State=10 DO IF (%Var1%==0) Add2 3600; Power1 1 ENDIF ENDON
ON Button1#State=11 DO IF (%Var1%==0) Add2 7200; Power1 1 ENDIF ENDON
ON Button1#State=12 DO IF (%Var1%==0) Add2 10800; Power1 1 ENDIF ENDON
ON Button1#State=13 DO IF (%Var1%==0) Add2 14400; Power1 1 ENDIF ENDON
ON Button1#State=14 DO IF (%Var1%==0) Add2 18000; Power1 1 ENDIF ENDON
ON Var2#state DO IF (%value%>0) RuleTimer1 %Var2%; RuleTimer2 60 ENDIF ENDON
ON Rules#Timer=1 DO Power1 0 ENDON
ON Rules#Timer=2 DO Sub2 60 ENDON
ON Rules#Timer=3 DO Power1 0 ENDON
ON Power1#State=1 DO RuleTimer3 60 ENDON
ON Power1#State=0 DO Backlog Var1 0; Var2 0; RuleTimer1 0; RuleTimer2 0 ENDON

Rule2
ON Energy#Power<10 DO RuleTimer3 60 ENDON
ON Energy#Power>10 DO RuleTimer3 0 ENDON
Rule2 5
