# encoder_kalman_filter

## System clock configuration
  โดยตั้งค่า base timer clock ไว้ที่ 100 MHz


## Timer
  timer2 ใช้เป็น encoder mode
        ตั้งค่า Counter Period  = 2400-1 =2399 

        2400 มาจาก encoder รุ่นนี้ 1 R = 300 PPR มี encoder มี 2 chanel A,B จะได้ว่า 300*2*4 = 2400
        เปิดการทำงาน global interrupt ของ timer2
        sConfig.IC1Polarity = TIM_ICPOLARITY_FALLING; เนื่องจาก encoder เป็นแบบ falling edge
        
        
  timer5 ใช้เป็น timer interrupt
  
        Prescaler = 99;
        Period = 999;
        เปิดการทำงาน global interrupt ของ timer5
        จะได้ว่าจาก base clock timer 100MHz โดน prescaler 100 จะเหลือ 1MHz 
        และ set period ที่ 100 CCR และเปิด global interrupt จะได้ ISR ทุกๆ 1000 microsec
  
  ## Include function
  ### GetRealdegree()
  return Degree ที่ผ่านการUnwrap
  ### GetRealrad()
  return Rad ที่ผ่านการUnwrap
  ### Unwarp()
  Unwrap โดยดูค่า encoder counter จาก register ของ timer2
 
  โดยดูการ Unwarp จาก ความแตกต่างของ encoder counter โดยตัดจากค่า Threshold Counter Period/2
  
  ดูสมการเต็มได้ที่ Scr/main.c Unwarp()
  
  ### Kalman()
  Kalman filterCode โดย ฟังก์ชั่นนี้จะถูกเรียนใน timer interrupt routine
  
  โดยสมการถูกปรับให้อยู่ในรูป Control loop โดยอ้างอิงจาก https://github.com/imchin/KalmanFilter_encoder
  
  
  
