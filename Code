#define rd 10
int avg(int16_t *arr){
  int ret=0;
  for(int i=0;i<rd;i++)
    ret+=arr[i];
  ret/=rd;
  return abs(ret);
}

void setup(){
  pinMode(5,OUTPUT);
  pinMode(6,OUTPUT);
  pinMode(7,OUTPUT);
  digitalWrite(5,HIGH);
  digitalWrite(7,LOW);
  Serial.begin(9600);
  Wire.begin();
  Wire.beginTransmission(MPU_addr);
  Wire.write(0x6B);  // PWR_MGMT_1 register
  Wire.write(0);     // set to zero (wakes up the MPU-6050)
  Wire.endTransmission(true);
}

void loop(){
  Wire.beginTransmission(MPU_addr);
  Wire.write(0x3B);  // starting with register 0x3B (ACCEL_XOUT_H)
  Wire.endTransmission(false);
  Wire.requestFrom(MPU_addr,6,true);  // request a total of 14 registers
  AcX[index]=Wire.read()<<8|Wire.read();  // 0x3B (ACCEL_XOUT_H) & 0x3C (ACCEL_XOUT_L)     
  AcY[index]=Wire.read()<<8|Wire.read();
  AcZ[index]=Wire.read()<<8|Wire.read();
  AcX[index]/=100;
  AcY[index]/=100;
  AcZ[index]/=100;
  devX=abs(avg(AcX)-AcX[index]);
  devY=abs(avg(AcY)-AcY[index]);
  devZ=abs(avg(AcZ)-AcZ[index]);
  index++;
  index%=rd;
  dev=sqrt(devX*devX+devY*devY+devZ*devZ);
  if(dev>6)
    buzzerDelay=1000;
    
  if(buzzerDelay){
    digitalWrite(6,LOW);
    buzzerDelay--;
  }
  else
    digitalWrite(6,HIGH);  
  Serial.println(dev);
  delay(10);
}
