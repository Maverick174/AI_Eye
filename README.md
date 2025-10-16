# 🧠 AI Eye — Real-time PPE Detection (CPU Edition)

**AI Eye** — это модуль компьютерного зрения, который интегрируется в систему видеонаблюдения и определяет наличие касок у людей в реальном времени.  
Проект оптимизирован для работы **на обычных ПК и мини-компьютерах без GPU**, использует YOLOv8n/s в CPU-режиме.

---

## ⚙️ Технологический стек

| Категория | Технологии | Комментарий |
|------------|-------------|-------------|
| **Модель** | YOLOv8n / YOLOv8s (CPU, PyTorch) | Используем лёгкую модель YOLO для ускоренного инференса. |
| **Видео** | OpenCV (RTSP / HTTP) | Потоковое видео через `cv2.VideoCapture`, обработка 1 из 3 кадров. |
| **API / Backend** | FastAPI | Лёгкий REST API-сервер `/video`, `/status`. |
| **Нотификации** | Telegram Bot API | Уведомления с изображениями при событиях отсутствия каски. |
| **Инфраструктура** | Docker, Linux (Ubuntu / Debian) | Контейнер без CUDA-библиотек, оптимизирован под CPU. |
| **Edge / Deployment** | Mini-PC, Intel NUC, Raspberry Pi 4/5 | Поддержка ARM и x86 CPU. |
| **Оптимизация (опц.)** | ONNX Runtime (CPU) | Ускоренный инференс без GPU, TensorRT исключён. |

---

## 🚀 Быстрый старт

### 1. Клонирование репозитория
```bash
git clone https://github.com/maverick174/ai-eye.git
cd ai-eye
2. Установка зависимостей
```bash
pip install -r requirements.txt
или вручную:
```bash
pip install ultralytics==8.2.1 fastapi uvicorn opencv-python-headless python-telegram-bot onnxruntime
3. Запуск backend
```bash
uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
4. Тест видео
```bash
python test_stream.py --source rtsp://camera_ip
🧩 Архитектура проекта
```graphql
📂 ai-eye/
├── app/
│   ├── main.py          # FastAPI endpoints /video, /status
│   ├── video_stream.py  # RTSP/HTTP поток
│   ├── detector.py      # YOLOv8n/s CPU инференс
│   └── notifier.py      # Telegram Bot API уведомления
├── models/
│   └── yolov8n.onnx     # Экспортированная модель (опц.)
├── requirements.txt
├── Dockerfile
└── README.md
Поток данных:
Камера → OpenCV → YOLOv8n (CPU) → FastAPI → Telegram Bot → Пользователь
🧱 Docker-сборка
1. Сборка контейнера
```bash
docker build -t ai-eye-cpu .
2. Запуск контейнера
```bash
docker run -d -p 8000:8000 --name ai-eye ai-eye-cpu
🧠 Контрольные точки
Этап	Описание	Цель
✅ 1	Подключен RTSP-поток	Поток стабильно обрабатывается
✅ 2	YOLOv8 CPU детектирует человека и каску	Модель работает на CPU
✅ 3	Telegram уведомления	Отправка сообщений при событиях
✅ 4	Docker сборка	Рабочая версия без CUDA
✅ 5	MVP продемонстрирован	Готов к питчу и тестированию

🗓 План разработки (CPU версия)
Неделя	Этап	Результат
13–17 окт	Подготовка окружения	Рабочее окружение
18–24 окт	Подключение RTSP/HTTP видеопотока	Стабильный поток
25–31 окт	Интеграция YOLOv8n (CPU)	Рабочий инференс
1–7 ноя	Разработка FastAPI backend	REST API
8–14 ноя	Подключение Telegram Bot API	Уведомления
15–21 ноя	Оптимизация ONNX Runtime (CPU)	Быстрый инференс
22–29 ноя	Тестирование и документация	Стабильный релиз
30 ноя–6 дек	Демонстрация MVP	Финальная презентация

📬 Контакты
AI Eye Development Team
Telegram: @maverick_174
Email: maverick174@mail.ru

🪪 Лицензия
Проект распространяется под лицензией Apache 2.0.
Вы можете использовать и модифицировать код с указанием авторства.
