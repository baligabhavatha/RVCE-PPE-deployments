# RVCE-PPE-deployments
# RVCE PPE Deployments

Azure IoT Edge deployment repository for Raspberry Pi based PPE Detection, RTSP Streaming, and GO/NO-GO Zone Monitoring.

---

# Repository Structure

```text
RVCE-PPE-deployments/
│
├── hello-world/
│   └── deployment.json
│
├── PPE/
│   ├── deployment.json
│   ├── deployment-rtsp.json
│   └── README.md
│
└── README.md
```

---

# Overview

This repository contains Azure IoT Edge deployment manifests for:

* Azure IoT Edge Hello World validation
* PPE Detection using YOLO
* RTSP Streaming support
* GO / NO-GO Zone Monitoring
* Streamlit frontend deployment
* Raspberry Pi ARM64 compatible containers

The deployment runs completely on Raspberry Pi using Azure IoT Edge Runtime and Docker containers.

---

# System Architecture

## Modules

| Module      | Description                         |
| ----------- | ----------------------------------- |
| edgeAgent   | Azure IoT Edge management module    |
| edgeHub     | Azure IoT Edge communication module |
| frontend    | Streamlit web UI                    |
| backend     | YOLO PPE detection backend          |
| rtsp-server | RTSP streaming container            |

---

# Requirements

## Hardware

* Raspberry Pi 4
* 64-bit Raspberry Pi OS (Bookworm)
* Minimum 4GB RAM recommended

---

## Software

* Docker
* Azure CLI
* Azure IoT Extension
* Azure IoT Edge Runtime

---

# Verify Raspberry Pi

```bash
cat /etc/os-release
uname -m
```

Expected architecture:

```bash
aarch64
```

---

# Verify Docker

```bash
docker version
docker run hello-world
```

---

# Verify Azure CLI

```bash
az version
az extension list -o table
```

---

# Verify IoT Edge Runtime

```bash
iotedge version
```

---

# Azure Login

```bash
az login --use-device-code
```

---

# Clone Repository

```bash
git clone https://github.com/baligabhavatha/RVCE-PPE-deployments.git

cd RVCE-PPE-deployments
```

---

# HELLO WORLD DEPLOYMENT

The `hello-world` deployment validates:

* Azure IoT Edge runtime
* Docker functionality
* Azure IoT Hub communication
* Deployment pipeline

---

## Deploy Hello World

```bash
cd hello-world

az iot edge set-modules \
 --hub-name edge-ai \
 --device-id rpi-edge-device \
 --content deployment.json
```

---

## Verify Modules

```bash
sudo iotedge list
```

Expected:

```text
edgeAgent
edgeHub
hello-world
```

---

## Check Logs

```bash
sudo iotedge logs hello-world -f
```

Expected:

```text
Hello from Azure IoT Edge!
```

---

# PPE DEPLOYMENT

The PPE deployment contains:

* Streamlit frontend
* YOLO backend inference
* Video upload support
* RTSP support
* GO / NO-GO zone monitoring

---

# Deploy PPE Application

```bash
cd PPE

az iot edge set-modules \
 --hub-name edge-ai \
 --device-id rpi-edge-device \
 --content deployment.json
```

---

# Verify PPE Modules

```bash
sudo iotedge list
```

Expected:

```text
edgeAgent
edgeHub
frontend
backend
```

---

# Access Streamlit Frontend

Get Raspberry Pi IP:

```bash
hostname -I
```

Open browser:

```text
http://<RASPBERRY_PI_IP>:8501
```

---

# RTSP STREAMING DEPLOYMENT

The RTSP deployment adds an additional RTSP server container.

This enables:

* Live RTSP streaming
* IP camera integration
* Real-time inference
* Low-latency processing

---

# Deploy RTSP Configuration

```bash
cd PPE

az iot edge set-modules \
 --hub-name edge-ai \
 --device-id rpi-edge-device \
 --content deployment-rtsp.json
```

---

# Verify RTSP Container

```bash
sudo iotedge list
docker ps
```

Expected modules:

```text
frontend
backend
rtsp-server
```

---

# RTSP Stream URL

```text
rtsp://<RASPBERRY_PI_IP>:8554/stream
```

Use this URL inside the Streamlit frontend.

---

# Supported Video Sources

| Source Type        | Supported |
| ------------------ | --------- |
| Uploaded MP4 Video | Yes       |
| RTSP Stream        | Yes       |
| IP Camera          | Yes       |
| USB Camera         | Yes       |

---

# GO / NO-GO ZONE MONITORING

The backend supports:

* Region of Interest (ROI)
* No-Go restricted zones
* Entry counting
* Real-time alerts
* Privacy blur
* Pixelation

---

# Features

## PPE Detection

* Helmet detection
* Vest detection
* No-helmet alerts
* No-vest alerts

---

## Zone Monitoring

* ROI counting
* Restricted area alerts
* Real-time overlays
* Red alert banners

---

# Update ROI Coordinates

Edit deployment manifest:

```bash
nano ~/RVCE-PPE-deployments/PPE/deployment.json
```

Update ROI and NO-GO coordinates.

Re-deploy:

```bash
az iot edge set-modules \
 --hub-name edge-ai \
 --device-id rpi-edge-device \
 --content deployment.json
```

---

# Backend Ports

| Service            | Port |
| ------------------ | ---- |
| Streamlit Frontend | 8501 |
| Backend API        | 8009 |
| RTSP Server        | 8554 |

---

# Useful Commands

## List Modules

```bash
sudo iotedge list
```

---

## Frontend Logs

```bash
sudo iotedge logs frontend -f
```

---

## Backend Logs

```bash
sudo iotedge logs backend -f
```

---

## RTSP Logs

```bash
docker logs <rtsp_container_id>
```

---

## Restart IoT Edge

```bash
sudo systemctl restart aziot-edged
```

---

# Troubleshooting

## Modules Stuck in Pulling

Check internet:

```bash
ping 8.8.8.8
```

---

## Frontend Not Accessible

Check:

```bash
sudo iotedge logs frontend -f
```

Verify port:

```bash
sudo netstat -tlnp | grep 8501
```

---

## Backend Crash

Check:

```bash
sudo iotedge logs backend -f
```

---

## RTSP Not Working

Verify port 8554 exposed:

```bash
docker ps
```

---

# Final Expected Result

You should see:

```text
edgeAgent
edgeHub
frontend
backend
rtsp-server
```

The Streamlit UI should open successfully in browser and support:

* Video upload
* RTSP streaming
* PPE detection
* GO/NO-GO monitoring
* Live alerts

---

# Technologies Used

* Azure IoT Edge
* Azure IoT Hub
* Azure Container Registry
* Docker
* Streamlit
* FastAPI
* OpenCV
* Ultralytics YOLO
* FFmpeg
* Raspberry Pi ARM64

---

# Author

RVCE Edge AI Lab
Azure IoT Edge PPE Deployment
