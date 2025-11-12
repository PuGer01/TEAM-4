#include <Servo.h>

// Определяем пины
const int servoPin = 9;     // Пин для сервопривода
const int buttonPin = 2;    // Пин для кнопки

Servo myServo;              // Создаем объект сервопривода

// Переменные для управления сервоприводом непрерывного вращения
const int stopSpeed = 90;   // Значение для остановки сервопривода
const int forwardSpeed = 0; // Скорость для движения вперед (по часовой)
const int backwardSpeed = 180; // Скорость для движения назад (против часовой)

// Переменные для контроля вращения
bool isRotating = false;
bool wasButtonPressed = false;
unsigned long rotationStartTime = 0;

// Параметры поворота на 90 градусов
const int targetAngle = 90; // Целевой угол в градусах
const int rotationSpeed = 72; // Скорость вращения (град/сек) - нужно калибровать
unsigned long rotationTime; // Время для поворота на 90 градусов

void setup() {
  myServo.attach(servoPin);           // Подключаем сервопривод к 9 пину
  pinMode(buttonPin, INPUT_PULLUP);   // Подключаем кнопку ко 2 пину с подтягивающим резистором
  myServo.write(stopSpeed);           // Останавливаем сервопривод при запуске
  
  // Рассчитываем время для поворота на 90 градусов
  rotationTime = (targetAngle * 1000) / rotationSpeed;
}

void loop() {
  // Считываем состояние кнопки
  bool buttonState = digitalRead(buttonPin);
  
  // Обработка нажатия кнопки
  if (buttonState == LOW && !wasButtonPressed) {
    // Кнопка только что нажата
    startRotation();
    wasButtonPressed = true;
  }
  
  // Обработка отпускания кнопки
  if (buttonState == HIGH && wasButtonPressed) {
    // Кнопка только что отпущена
    returnToStart();
    wasButtonPressed = false;
  }
  
  // Проверка завершения вращения вперед
  if (isRotating && millis() - rotationStartTime >= rotationTime) {
    myServo.write(stopSpeed); // Останавливаем сервопривод
    isRotating = false;
  }
  
  delay(10); // Небольшая задержка для стабильности
}

void startRotation() {
  rotationStartTime = millis(); // Запоминаем время начала вращения
  isRotating = true;
  myServo.write(forwardSpeed); // Начинаем вращение вперед
}

void returnToStart() {
  // Если сервопривод все еще вращается вперед, останавливаем его
  if (isRotating) {
    myServo.write(stopSpeed);
    isRotating = false;
  }
  
  // Вращаемся назад в течение того же времени
  myServo.write(backwardSpeed);
  delay(rotationTime); // Ждем, пока сервопривод вернется в исходное положение
  myServo.write(stopSpeed); // Останавливаем сервопривод
}
