# Intraday Trading Compass - Backend

[![Node.js](https://img.shields.io/badge/Node.js-18%2B-green)](https://nodejs.org/)
[![InfluxDB](https://img.shields.io/badge/InfluxDB-OSS%203.x-blue)](https://www.influxdata.com/)
[![License](https://img.shields.io/badge/License-MIT-yellow)](LICENSE)

## Overview
This repository contains the backend implementation for the Intraday Trading Compass, a web-based platform designed for intraday traders focusing on Nifty options and futures. It provides systematic, scenario-based trading signals with real-time data ingestion, indicator calculations (e.g., CPR, EMAs, VWAP), signal generation (e.g., ORB, NR7, CPR Breakouts), and streaming capabilities.

The backend is built with Node.js, using InfluxDB as the time-series database for efficient storage and querying of market data, indicators, and signals. It includes REST APIs for historical data, WebSocket streaming for real-time updates, and scheduled ingestion from market data providers.

This project follows a phased roadmap to ensure modular development, starting from core scaffolding to advanced features like authentication and monitoring.

**Key Goals:**
- Automate market data fetching and processing for Nifty symbols.
- Generate context-aware trading signals based on predefined scenarios.
- Provide secure, real-time data streaming to frontend clients.
- Support customization for user preferences and risk management.

## Features
- **Data Ingestion**: Scheduled fetching of OHLC candles from NSE or Alpha Vantage; computation of indicators like CPR, EMAs (9/21/50/200), ATR, and VWAP.
- **Signal Generation**: Detection of 25+ scenarios (e.g., CPR-Bullish/Breakout, EMA Bounce/Crossover, ORB, NR7, Gap Up/Down, Volume Breakouts) with confluence checks.
- **Time-Series Storage**: InfluxDB buckets for high-resolution (30-day) and downsampled (365-day) data; measurements for candles, indicators, and signals.
- **REST APIs**: Endpoints for historical candles, signals, and user settings; with pagination, rate-limiting, and authentication.
- **Real-Time Streaming**: WebSocket server for pushing new candles, indicators, and signals to subscribed clients.
- **Error Handling & Monitoring**: Retry logic, logging, and basic metrics integration (e.g., Prometheus/Grafana).
- **Security**: API key/JWT authentication for HTTP and WebSocket endpoints.
- **Deployment**: Dockerized for easy local/dev setup and on-prem deployment.

## Tech Stack
- **Runtime**: Node.js 18+
- **Framework**: Express.js (or Fastify for performance)
- **Database**: InfluxDB OSS 3.x (time-series optimized)
- **Scheduling**: node-cron (for ingestion tasks)
- **WebSockets**: ws or socket.io
- **Other Libraries**: @influxdata/influxdb-client-node, dotenv, express-rate-limit
- **Deployment**: Docker, docker-compose
- **Monitoring**: Prometheus + Grafana (optional)

## Setup Instructions
### Prerequisites
- Node.js 18+ installed
- Docker (for InfluxDB and app containerization)
- Environment variables: Create a `.env` file with keys like `INFLUX_URL`, `INFLUX_TOKEN`, `INFLUX_ORG`, `DATA_PROVIDER_API_KEY` (e.g., for Alpha Vantage)

### Local Development
1. Clone the repo:
   git clone https://github.com/medeepu/intraday-trading-compass.git
   cd intraday-trading-compass

2. Install dependencies:
   npm install

3. Set up InfluxDB:
- Run InfluxDB via Docker: `docker run -d -p 8086:8086 influxdb:latest`
- Access the UI at http://localhost:8086 and create an organization, buckets (`highres`, `downsampled`), and API token.
- Update `.env` with your InfluxDB credentials.

4. Start the server:
  npm start

- Health check: Visit http://localhost:3000/health

### Docker Deployment
1. Build and run with docker-compose:
  docker-compose up -d

For production, deploy on a server (e.g., AWS EC2, DigitalOcean) using systemd or as a Docker service. See `deployment-guide.md` for details (to be added in Phase 7).

## Roadmap & Phases
The development follows a phased approach as outlined below. Each phase includes specific tasks, with progress tracked via GitHub issues/milestones.

### Phase 1: Project Scaffold & Core Services
- Initialize Node.js project with Express.
- Set up InfluxDB instance and client.
- Basic health-check endpoint.

### Phase 2: Historical Data & REST API
- Endpoints for fetching candles and signals via Flux queries.
- Pagination and rate-limiting.

### Phase 3: Market Data Ingestion
- Integrate NSE/Alpha Vantage for candle fetching.
- Compute and write indicators (CPR, EMAs, ATR).
- Error handling and logging.

### Phase 4: Signal-Generation Engine
- Detect trading scenarios (ORB, NR7, CPR Breakouts, etc.).
- Write signals to InfluxDB.

### Phase 5: Real-Time Streaming
- WebSocket server for live updates.
- Event emitters for new data.

### Phase 6: Authentication & User Settings
- API key auth.
- Persist user preferences (e.g., symbols, intervals).

### Phase 7: Documentation & Deployment
- OpenAPI spec and deployment scripts.
- Monitoring with Prometheus/Grafana.

Current Status: Starting Phase 1. Track progress in the [Issues](https://github.com/medeepu/intraday-trading-compass/issues) tab.

## API Documentation
(Coming in Phase 7: OpenAPI spec with examples.)

Example: GET /api/candles?symbol=NIFTY&interval=5m&start=2025-07-01&end=2025-07-20
