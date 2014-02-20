Flight Controller based on K20DX256 (Teensy 3.1)
------------------------------------------------

PIN setup (Teensy 3.1 pin numbering)
------------------------------------
  - I2C SCL 19
  - I2C SDA 18
  
  - SUMD in (receiver) 0
  
  - Rotor 1 22
  - Rotor 2 23
  - Rotor 3 9
  - Rotor 4 10
  - Rotor 5 6
  - Rotor 6 20
  - Rotor 7 21
  - Rotor 8 5
  
  - Orientation lights / Armed-Disarmed indicator 14
  
  - Battery Monitor (current sensor) 17
  
Filters, kinematics, data handling
----------------------------------
  - Initial raw data from sensors (read every 1ms = 1000Hz) is being averaged by a simple averaging filter
  - Averaged data from sensors are being processed every 10ms (100Hz)
  - CMP kinematitcs (my own) selected by default
  - ARG kinematics (from aeroquad) is also supported (can be enabled by simple include change)
  - DCM kinematics (from FreeIMU)
  - Flight controller supports
    - Rate (ACRO) | gyro only
    - Attitude | gyro with accel corrections
    - Altitute hold | barometer or sonar
  - Pilot commands are being read via single PIN (with HW timer or USART), PPM, PWM and SUMD in is supported
  - Stabilization and pilot commands are mixed together by 2 separate PID controllers
    - First (only used in attitude mode) mixes pilot commands with kinematics output
    - Second (used in both attitude and rate mode) mixes output from first PID or raw stick input with gyroRate output
  - For ESC signal output i am using an build in 8 channel FLEX timer (yes you can controll octocopter with this)
    - ESC PWM signal supports both 250Hz and 400Hz update rate (running at 400Hz by default)

Accelerometer offset trimming via TX
------------------------------------
  - While in dis-armed state
  - To enter the trimming mode = Throttle stick UP & Rudder LEFT
  - To adjust the values = PITCH UP & DOWN || ROLL LEFT & RIGHT
  - To save the values in EEPROM = Throttle UP & Rudder RIGHT 
    - (you dont have to save the values right away, you can test them, then return back to trimming mode and save them)
  - To leave trimming mode = Throttle stick DOWN