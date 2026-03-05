# CrowdWatch — AI Crowd Density Analysis & Congestion Prediction System

An AI-powered crowd monitoring web application that uses YOLOv8 to detect people in real-time from camera feeds, calculates crowd density, classifies congestion levels, generates alerts, stores data, and sends email notifications to authority personnel.

## Team
- C Hridya Ajay (ASI23CS071)
- Dayana Shiju (ASI23CS075)  
- Gagana Parvathi (ASI23CS088)
- Submitted to: Ms Raghi R Menon

## Features

- **Real-time Crowd Detection**: Uses YOLOv8n model to detect people from camera feeds
- **Density Calculation**: Calculates people per square meter in defined zones
- **Congestion Classification**: Low (< 5 people), Medium (≥ 5), High (≥ 10)
- **Alert System**: Automatic dashboard alerts and email notifications
- **Bus Monitoring**: Special mode for tracking bus entry/exit with line crossing detection
- **Multi-zone Support**: 5 simulated zones with individual crowd tracking
- **Analytics Dashboard**: Historical data visualization with Chart.js
- **Mobile Responsive**: Works on all devices with bottom navigation

## Tech Stack

- **Backend**: Python, Flask, Flask-CORS
- **AI Model**: YOLOv8n (Ultralytics)
- **Computer Vision**: OpenCV
- **Database**: SQLite (`crowd.db`)
- **Email**: Gmail SMTP with App Passwords
- **Frontend**: HTML5, CSS3, JavaScript (Vanilla)
- **Charts**: Chart.js
- **Icons**: Font Awesome
- **Fonts**: Google Fonts (Poppins, Space Mono)

## Quick Start

### 1. Install Dependencies
```bash
pip install -r requirements.txt
```

### 2. Configure Email (Optional)
Edit `app.py` and update these settings:
```python
EMAIL_USER = "your_email@gmail.com"
EMAIL_PASSWORD = "your_16_char_app_password"  # Gmail App Password
EMAIL_RECIPIENTS = [
    "recipient1@gmail.com",
    "recipient2@gmail.com", 
    "recipient3@gmail.com"
]
```

### 3. Run the Application
```bash
python app.py
```

### 4. Access the System
- **Main Application**: http://localhost:5000
- **Login**: Use any credentials (prototype mode)
- **Standalone Demo**: Open `prototype.html` directly in browser (no server needed)

## Pages & Features

### 1. Login (`index.html`)
- Simple authentication gateway
- Any credentials accepted for prototype

### 2. Dashboard (`dashboard.html`)
- Real-time crowd statistics
- Alert status overview
- Quick navigation to all features
- Recent alerts list

### 3. Live Monitor (`live.html`)
- Live MJPEG video stream with YOLOv8 detections
- Real-time people count and density
- System information display

### 4. Bus Monitor (`busmonitor.html`)
- Toggle between Normal and Bus modes
- Track passengers entering/exiting bus
- Occupancy percentage with color-coded progress bar
- Live passenger trend chart

### 5. Zone Map (`map.html`)
- 5-zone facility visualization
- Real-time zone status with color coding
- Animated markers for high congestion
- Zone list with detailed statistics

### 6. Analytics (`analytics.html`)
- Historical crowd data visualization
- Line chart: Last 50 readings
- Doughnut chart: Congestion level distribution  
- Bar chart: 24-hour hourly averages
- Summary statistics (current, peak, average)

### 7. Alert Center (`alerts.html`)
- Alert history with acknowledgment
- Email notification status
- Test email functionality
- Unacknowledged alert tracking

### 8. Standalone Prototype (`prototype.html`)
- Complete self-contained demo
- Simulated crowd data with realistic patterns
- All features in single HTML file
- No server required

## Configuration

### Camera Settings
```python
CAMERA_SOURCE = 0  # 0 = webcam, or IP camera URL
ZONE_AREA = 50.0   # square metres per zone
```

### Detection Thresholds
```python
THRESHOLD_MED = 5   # Medium congestion threshold
THRESHOLD_HIGH = 10 # High congestion threshold
BUS_CAPACITY = 45   # Bus capacity for occupancy tracking
```

### Email Alerts
- **Medium Cooldown**: 120 seconds
- **High Cooldown**: 60 seconds
- **Recipients**: Up to 3 authority personnel
- **Format**: Styled HTML with crowd statistics

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/video` | MJPEG live stream with detections |
| GET | `/api/crowd` | Current crowd statistics |
| GET | `/api/alerts/history` | Complete alert history |
| GET | `/api/history` | Last 50 crowd records |
| GET | `/api/analytics` | Hourly aggregated data |
| GET | `/api/zones` | 5-zone crowd data |
| GET | `/api/set_mode/<mode>` | Switch detection mode |
| POST | `/api/acknowledge/<id>` | Acknowledge alert |
| POST | `/api/email/test` | Send test email |
| GET | `/api/status` | System health check |

## Database Schema

### crowd_history
```sql
CREATE TABLE crowd_history (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    timestamp TEXT,
    count INTEGER,
    density REAL,
    level TEXT
);
```

### alerts
```sql
CREATE TABLE alerts (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    timestamp TEXT,
    level TEXT,
    message TEXT,
    people_count INTEGER,
    acknowledged INTEGER DEFAULT 0,
    email_sent INTEGER DEFAULT 0
);
```

## Gmail App Password Setup

1. Enable 2-Step Verification on your Google Account
2. Go to Security → App Passwords
3. Create new app password named "CrowdWatch"
4. Copy the 16-character password
5. Update `EMAIL_PASSWORD` in `app.py`

## File Structure
```
crowdwatch/
├── app.py                 # Flask backend with YOLOv8
├── requirements.txt       # Python dependencies
├── yolov8n.pt            # YOLOv8 model file
├── crowd.db              # SQLite database
├── index.html            # Login page
├── dashboard.html        # Main dashboard
├── live.html             # Live video monitoring
├── busmonitor.html       # Bus entry/exit tracking
├── map.html              # Zone visualization
├── analytics.html        # Data analytics
├── alerts.html           # Alert management
├── prototype.html        # Standalone demo
├── static/
│   ├── style.css         # Global styles
│   └── app.js            # Shared JavaScript
└── README.md             # This file
```

## Browser Support

- Chrome 80+
- Firefox 75+
- Safari 13+
- Edge 80+

## Security Notes

- Prototype accepts any login credentials
- Email passwords should use App Passwords, not account passwords
- Camera feed is accessible on local network only
- No user authentication in prototype mode

## Troubleshooting

### Camera Not Working
- Check if webcam is connected and not used by other apps
- Try changing `CAMERA_SOURCE` to different index or IP URL
- Ensure OpenCV can access the camera

### Email Not Sending
- Verify Gmail App Password is correctly set
- Check if 2-Step Verification is enabled
- Ensure recipient emails are valid

### High CPU Usage
- YOLOv8n is optimized but still CPU-intensive
- Consider reducing video resolution or frame rate
- Use GPU acceleration if available

## Performance

- **Detection Response**: < 2 seconds
- **Update Frequency**: 2 seconds for UI, 1 second for video
- **Database Logging**: Every 30 frames (~1 second)
- **Memory Usage**: ~500MB (including model)

## License

This project is for educational purposes. Use responsibly and comply with local privacy laws when monitoring public spaces.

---

**CrowdWatch** - Making public spaces safer through AI-powered crowd monitoring 🚶‍♂️📊 
