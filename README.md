# Wall-Detection-Mode

#define MAX_DISTANCE 200

// 전역 변수
int mode = -1;
int UltrasonicSensorData[4];

// 함수 선언
void Sonar_Data_Display(int flag);
void Robot_Mode_Define(void);

void setup() {
  Serial.begin(9600); // Serial 통신 초기화
}

void loop() {
  // 메인 코드 실행
  Robot_Mode_Define();
  Serial.println(mode);

  // wall_following_c(12); // 필요한 경우 주석 해제
}

// 초음파 센서 데이터 출력 함수
void Sonar_Data_Display(int flag) {
  char Sonar_data_display[40];
  if (flag == 0) return;
  else {
    sprintf(Sonar_data_display, "F:");
    Serial.print(Sonar_data_display);
    Serial.print(UltrasonicSensorData[0]);
    Serial.print("cm B:");
    Serial.print(UltrasonicSensorData[1]);
    Serial.print("cm R:");
    Serial.print(UltrasonicSensorData[2]);
    Serial.print("cm L:");
    // 왼쪽 센서 데이터가 200을 초과하는 경우 200으로 제한
    if (UltrasonicSensorData[3] > MAX_DISTANCE) {
      Serial.print(MAX_DISTANCE);
    } else {
      Serial.print(UltrasonicSensorData[3]);
    }
    Serial.println("cm"); // 왼쪽 센서의 값 출력 후 "cm" 단위 추가
  }
}

// 로봇 모드 정의 함수
void Robot_Mode_Define(void) {
  int i;
  // 초음파 센서 데이터 읽기
  // read_ulreasonic_sensor(); // 필요한 경우 호출
  for (i = 0; i < 4; i++) {
    if (UltrasonicSensorData[i] == 0) UltrasonicSensorData[i] = MAX_DISTANCE;
  }
  Sonar_Data_Display(1);

  // mode == 0
  if ((UltrasonicSensorData[2] >= 15) && (UltrasonicSensorData[3] >= 15)) {
    mode = 0;
  }
  // mode == 1
  if ((UltrasonicSensorData[2] <= 15) && (UltrasonicSensorData[3] <= 15)) {
    mode = 1;
  }
  // mode == 2
  if ((UltrasonicSensorData[3] <= 35) && (UltrasonicSensorData[2] >= 40)) {
    mode = 2;
  }
  // mode == 3
  if ((UltrasonicSensorData[2] <= 35) && (UltrasonicSensorData[3] >= 40)) {
    mode = 3;
  }
}
