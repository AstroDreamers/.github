<div align="center">

<h1>🌍 AstroDreamers</h1>

</div>
![NASA TEMPO](https://img.shields.io/badge/NASA-TEMPO-blue?style=for-the-badge)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.5.6-brightgreen?style=for-the-badge&logo=springboot)
![React](https://img.shields.io/badge/React-19.1-61DAFB?style=for-the-badge&logo=react)
![FastAPI](https://img.shields.io/badge/FastAPI-0.109-009688?style=for-the-badge&logo=fastapi)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-17.6-316192?style=for-the-badge&logo=postgresql)

**Real-Time Air Quality Monitoring & Predictive Alert System**

Built for the 48-hour NASA Space Apps Challenge 2025

[Live Demo](https://astrodreamers.netlify.app) · [API Documentation](https://tempo-backend-rzn2.onrender.com/swagger-ui)

</div>

---

## 🌟 Overview

AstroDreamers transforms NASA TEMPO satellite data and ground-based sensors into actionable health information. Monitor air quality across multiple locations, track 6 key pollutants (SO₂, NO₂, PM2.5, PM10, O₃, CO), and receive personalized alerts when pollution exceeds safe thresholds.

**The Impact**: Air pollution causes 7 million deaths annually. We democratize air quality monitoring, empowering communities to make informed decisions and advocate for cleaner air.

---

## ✨ Key Features

- 🔐 **Secure Authentication** - JWT-based auth with refresh tokens
- 📍 **Multi-Location Monitoring** - Subscribe to unlimited locations worldwide
- 🚨 **Smart Alerts** - Customizable thresholds with quiet hours (10 PM - 8 AM)
- 📊 **Real-Time Visualization** - Interactive charts and color-coded AQI indicators
- 🤖 **ML Predictions** - 6-24 hour forecasts using LightGBM & Random Forest models
- 📧 **Email Notifications** - Instant alerts when pollution exceeds your limits
- 🗺️ **Interactive Maps** - Leaflet-powered location search and visualization

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                   Frontend (React 19.1)                      │
│         Tailwind CSS · Leaflet Maps · Recharts              │
└────────────────────┬────────────────────────────────────────┘
                     │ REST API
                     │
┌────────────────────▼────────────────────────────────────────┐
│              Backend (Spring Boot 3.5.6)                     │
│  Authentication · Subscriptions · Alerts · Email Service    │
└────────┬───────────────────────────────────────┬────────────┘
         │                                       │
         │ HTTP Request                          │ JPA/Hibernate
         │                                       │
┌────────▼────────────────┐           ┌─────────▼─────────────┐
│   ML Service (FastAPI)  │           │  PostgreSQL Database  │
│  LightGBM · Random      │           │  Users · Subscriptions│
│  Forest · scikit-learn  │           │  Alerts · Predictions │
└─────────────────────────┘           └───────────────────────┘
         │
         │ Data Feed
         │
┌────────▼─────────────────────────────────────────────────────┐
│                    External APIs                              │
│         OpenAQ Ground Sensors · NASA TEMPO Satellite         │
└───────────────────────────────────────────────────────────────┘
```

**Flow**: Frontend → Spring Boot → FastAPI (ML predictions) → PostgreSQL + External APIs

---

## 🛠️ Technology Stack

### Backend
- **Spring Boot 3.5.6** - REST API, authentication, business logic
- **Spring Security + JWT** - Authentication & authorization
- **PostgreSQL 17.6** - Data persistence
- **Spring Mail** - Email notifications

### ML Service
- **FastAPI** - High-performance Python API
- **LightGBM** - Gradient boosting for time-series forecasting
- **Random Forest** - Ensemble learning for PM2.5 predictions
- **scikit-learn** - Model training and evaluation
- **pandas/numpy** - Data processing

### Frontend
- **React 19.1** - Modern UI with concurrent features
- **Tailwind CSS 3.4** - Utility-first styling
- **Leaflet + MapLibre GL** - Interactive maps
- **Recharts 3.2** - Data visualization
- **Axios** - API communication

### DevOps
- **Docker** - Containerization
- **Render.com** - Backend & ML hosting
- **Netlify** - Frontend hosting with CDN

---

## 📁 Project Structure

```
AstroDreamers/
├── tempo-backend/              # Spring Boot REST API
│   ├── controller/            # REST endpoints
│   ├── service/               # Business logic + ML service client
│   ├── model/                 # JPA entities
│   └── repository/            # Data access
│
├── tempo-ml-service/          # FastAPI ML microservice
│   ├── models/                # LightGBM & Random Forest models
│   ├── api/                   # FastAPI endpoints
│   ├── training/              # Model training scripts
│   └── requirements.txt       # Python dependencies
│
└── tempo-frontend/            # React SPA
    ├── components/            # Reusable UI components
    ├── pages/                 # Route pages
    └── api/                   # API integration
```

---

## 🚀 Live Deployment

| Service | URL | Status |
|---------|-----|--------|
| **Frontend** | [astrodreamers.netlify.app](https://astrodreamers.netlify.app) | ✅ Live |
| **Backend API** | [tempo-backend-rzn2.onrender.com](https://tempo-backend-rzn2.onrender.com) | ✅ Live |
| **API Docs** | [tempo-backend-rzn2.onrender.com/swagger-ui](https://tempo-backend-rzn2.onrender.com/swagger-ui) | ✅ Live |
| **ML Service** | Internal microservice | ✅ Live |

---

## 🎯 Getting Started

### Backend
```bash
git clone https://github.com/AstroDreamers/tempo-backend.git
cd tempo-backend

# Configure .env
DATABASE_URL=jdbc:postgresql://localhost:5432/tempo
JWT_SECRET_KEY=your-secret-key
ML_SERVICE_URL=http://localhost:8001

./mvnw spring-boot:run
# Runs on http://localhost:8080
```

### ML Service
```bash
git clone https://github.com/AstroDreamers/tempo-ml-lab.git
cd tempo-ml-lab

pip install -r requirements.txt
uvicorn main:app --reload --port 8001
# Runs on http://localhost:8001
```

### Frontend
```bash
git clone https://github.com/AstroDreamers/tempo-frontend.git
cd tempo-frontend

npm install
REACT_APP_API_URL=http://localhost:8080 npm start
# Runs on http://localhost:3000
```

---

## 💡 ML Prediction System

### How It Works
1. **Data Collection**: Historical PM2.5 data from OpenAQ + NASA TEMPO
2. **Feature Engineering**: Time-based features, rolling averages, weather data
3. **Model Training**: LightGBM & Random Forest on 6-12 months of data
4. **Ensemble Prediction**: Weighted averaging of both models
5. **Real-Time Forecasting**: 6-hour and 24-hour predictions updated hourly

### API Integration
Spring Boot calls FastAPI endpoint for predictions:
```java
// Spring Boot Service
@Service
public class MLService {
    public PredictionResponse getPrediction(String locationId) {
        return restTemplate.postForObject(
            mlServiceUrl + "/predict",
            request,
            PredictionResponse.class
        );
    }
}
```

**Status**: Phase 2 - Currently in development

---

## 📊 Key Endpoints

### Authentication
- `POST /auth/signup` - Register user
- `POST /auth/login` - Login with JWT

### Subscriptions
- `GET /subscriptions` - List all subscriptions
- `POST /subscriptions` - Subscribe to location
- `DELETE /subscriptions/{id}` - Unsubscribe

### Alerts
- `GET /subscriptions/{id}/alerts` - List alerts
- `POST /subscriptions/{id}/alerts` - Create alert
- `PATCH /subscriptions/{id}/alerts/{sensorId}/enable` - Enable alert

### ML Predictions (FastAPI)
- `POST /predict` - Get 6-24 hour forecasts
- `GET /model/metrics` - Model performance stats

---

## 🏆 Technical Achievements

✅ **Full-Stack Development** - React, Spring Boot, FastAPI, PostgreSQL  
✅ **Microservices Architecture** - Decoupled ML service  
✅ **Enterprise Security** - JWT auth, BCrypt hashing, CORS  
✅ **Machine Learning** - Time-series forecasting models  
✅ **Cloud Deployment** - Docker, Render, Netlify  
✅ **External API Integration** - OpenAQ, NASA TEMPO  
✅ **Real-Time Notifications** - Email alerts with SMTP  
✅ **Responsive Design** - Mobile-first UI with Tailwind  

---

## 🎓 Skills Demonstrated

**Backend**: Spring Boot, Spring Security, JPA/Hibernate, RESTful APIs, JWT  
**ML/AI**: Python, FastAPI, LightGBM, Random Forest, scikit-learn, pandas  
**Frontend**: React 19, Tailwind CSS, Leaflet Maps, Axios, Recharts  
**Database**: PostgreSQL, SQL, database design  
**DevOps**: Docker, cloud deployment, environment management  
**Architecture**: Microservices, REST APIs, MVC pattern, clean code  

---

## 📝 Future Enhancements

- [ ] Expand ML predictions to all 6 pollutants
- [ ] Mobile app with push notifications
- [ ] Community features for shared alerts
- [ ] Integration with health/fitness devices
- [ ] Advanced analytics dashboard
- [ ] Multi-language support

---

## 🔗 Links

- 🌐 [Live Application](https://astrodreamers.netlify.app)
- 📚 [API Documentation](https://tempo-backend-rzn2.onrender.com/swagger-ui)
- 💻 [Backend Repo](https://github.com/AstroDreamers/tempo-backend)
- 🤖 [ML Service Repo](https://github.com/AstroDreamers/tempo-ml-lab)
- ⚛️ [Frontend Repo](https://github.com/AstroDreamers/tempo-frontend)

---

<div align="center">

### Built for NASA Space Apps Challenge 2025 🚀

**Transforming satellite data into life-saving information**

⭐ Star this repo if you find it helpful!

</div>
