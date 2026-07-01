# Sense My Rally

Sense My Rally is a local-first tennis video analysis app for personal use. The long-term goal is to let a player record rallies on a phone, analyse the video locally, and receive useful coaching-style feedback.

This project is integration-first. We are not trying to reinvent every tennis computer vision model from scratch. Instead, the app is designed around clean adapters for mature open source tools such as court detectors, TrackNet-style ball tracking, YOLO/ByteTrack player tracking, and MediaPipe Pose.

## Current MVP

The first version includes:

- FastAPI backend
- `GET /health`
- `POST /analyse-video`
- OpenCV video metadata extraction
- Placeholder adapters for court detection, ball tracking, player tracking, and pose estimation
- Structured rally analysis JSON
- Expo React Native mobile shell
- Reusable mobile-first UI components
- Docker Compose setup
- GitHub Actions smoke test for the API

## Repository structure

```text
apps/
  api/
    app/
      main.py
      models/
        schemas.py
      services/
        video_metadata.py
        pose_analysis.py
        rally_analysis.py
      adapters/
        court_detector.py
        ball_tracker.py
        player_tracker.py
        pose_estimator.py
    requirements.txt
    Dockerfile
  mobile/
    app/
      index.tsx
    components/
      AnalysisCard.tsx
      StatCard.tsx
      UploadButton.tsx
    package.json
docs/
  technical-plan.md
  open-source-strategy.md
  sample-api-response.json
docker-compose.yml
.github/
  workflows/
    api-smoke-test.yml
```

## API setup

From the repo root:

```bash
cd apps/api
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

Test the health endpoint:

```bash
curl http://localhost:8000/health
```

Analyse a local video:

```bash
curl -X POST http://localhost:8000/analyse-video \
  -F "file=@/path/to/tennis-rally.mp4"
```

## Docker usage

```bash
docker compose up --build
```

The API will be available at:

```text
http://localhost:8000
```

## Mobile setup

From the repo root:

```bash
cd apps/mobile
npm install
npm run start
```

By default the mobile shell expects the API at:

```text
http://localhost:8000
```

When testing on a physical phone, replace that with your computer's LAN IP address in the mobile app.

## Roadmap

1. Stabilise the backend upload and metadata pipeline.
2. Plug in a real court detector.
3. Add tennis ball tracking through TrackNet or YOLO-based models.
4. Add player tracking with YOLO and ByteTrack.
5. Add MediaPipe Pose for body mechanics.
6. Build a rally event engine that detects hits, bounces, rally starts, and rally ends.
7. Generate useful player-facing stats and coaching summaries.
8. Improve the mobile app for session history, progress tracking, and video review.

## Design principle

The raw detections are not the product. The product is the coaching layer: turning video-derived data into clear, useful feedback for recreational tennis players.
