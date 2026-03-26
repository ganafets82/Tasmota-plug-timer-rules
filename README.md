# Tasmota-plug-timer-rules
Tasmota plug Timer Rules and auto off for 2kw Car charger (and other things)


# Var1: Always on mode (long button press, duration with SetOption32 in 1/10sec)
# Var2: Timer mode (activated with 1 or 2-5 short button presses)
# Var3: Timer value (adds 1h with every short button press) 
# Var4: Timeout activation for auto power off

# Maybe the other rules have to deactivated (Rule1 0; Rule2 0; ...)

# Turn off: Long button press, or power < 100W for 60s
# Timer: Short button presses activates Timer and/or adds 60h

SetOption73 1
SetOption32 10
ButtonTopic 0

Rule3
ON button1#state=3 DO Backlog Power1 2; Var1 1 ENDON
ON Button1#State=10 DO IF (%Var1%==0) Backlog Var2 1; Add3 3600; Power1 1 ENDIF ENDON
ON Button1#State=11 DO IF (%Var1%==0) Backlog Var2 1; Add3 7200; Power1 1 ENDIF ENDON
ON Button1#State=12 DO IF (%Var1%==0) Backlog Var2 1; Add3 10800; Power1 1 ENDIF ENDON
ON Button1#State=13 DO IF (%Var1%==0) Backlog Var2 1; Add3 14400; Power1 1 ENDIF ENDON
ON Button1#State=14 DO IF (%Var1%==0) Backlog Var2 1; Add3 18000; Power1 1 ENDIF ENDON
ON Var3#state DO IF (%Var2%==1) RuleTimer1 %Var3% ENDIF ENDON
ON Rules#Timer=1 DO Power1 0 ENDON
ON Energy#Power<100 DO IF (%Var4%==0) Backlog Var4 1; RuleTimer2 60 ENDIF ENDON
ON Energy#Power>100 DO Backlog Var4 0; RuleTimer2 0 ENDON
ON Rules#Timer=2 DO Power1 0 ENDON
ON Power1#State=0 DO Backlog Var1 0; Var3 0; Var4 0 ENDON
