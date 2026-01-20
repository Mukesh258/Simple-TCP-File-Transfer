# FT-Echo TCP File Transfer Project

A complete implementation of the **FT-Echo protocol** for TCP-based file transfer with support for:

- LIST  
- GET  
- PUT  
- RESUME  

Includes:

- Async TCP server  
- Synchronous client library  
- CLI client  
- FastAPI HTTP wrapper  
- React frontend  

Built for learning **Computer Networks**, socket programming, and real-world file transfer systems.

---

## 📁 Project Structure

ft-echo-project/  
├── README.md  
├── transcript.txt          # Test run transcript  
├── server/  
│   ├── tcp_server.py      # Async TCP FT-Echo server  
│   ├── tcp_client_lib.py  # Synchronous client library  
│   ├── cli_client.py      # CLI for manual testing  
│   ├── fastapi_app.py     # FastAPI HTTP wrapper  
│   ├── requirements.txt   # Python dependencies  
│   └── logs/              # Server logs  
├── frontend/  
│   ├── package.json  
│   └── src/               # React application  
├── demo_scripts/  
│   ├── run_tests.sh       # Automated test script  
│   └── demo_commands.txt  # Manual test commands  
└── storage/               # Server file storage  

---

## 📜 FT-Echo Protocol Specification

### Message Format

Every message follows this structure:

- 4 bytes: Big-endian uint32 length (N)  
- 1 byte: ASCII message type  
- (N - 1) bytes: Payload  

---

### Message Types

| Type | Meaning |
|------|--------|
| L | LIST |
| G | GET |
| P | PUT |
| R | RESUME |
| Q | QUIT |
| O | OK |
| E | ERROR |
| F | FILE |
| S | SHA256 |

---

## 🔁 Protocol Flow

### LIST (L)

Client:  
L  

Server:  
O + filename|size\n  

---

### GET (G)

Client:  
G + filename  

Server:  
O + {"size": N}  
F + file chunks  
S + SHA256 checksum  

---

### PUT (P)

Client:  
P + {"filename": "...", "size": N}  

Server:  
O + Ready to receive  

Client:  
F + file chunks  

Server:  
O + SHA256 checksum  

---

### RESUME (R)

Client:  
R + filename|offset|direction  

Server verifies `.part` file  
Transfer resumes from offset  

---

## 🛠 Installation

### Prerequisites

- Python 3.10+  
- Node.js 16+  
- npm  
- Bash (Unix-like systems)  

---

### Python Dependencies

cd server  
pip install -r requirements.txt  

---

### React Frontend

cd frontend  
npm install  

---

## ▶️ Usage

---

### 1. Start the TCP Server

Linux / macOS:

cd server  
python3 tcp_server.py [port]  

Windows:

cd server  
python tcp_server.py [port]  

Default port: 9000  

Server behavior:

- Listens on 0.0.0.0  
- Stores files in ../storage/  
- Logs to logs/server.log  

---

### 2. CLI Client

Linux / macOS:

cd server  
python3 cli_client.py  

Windows:

cd server  
python cli_client.py  

Commands:

connect <host> <port>  
list  
get <filename> [dest_path]  
put <filepath>  
resume <file> <offset> <direction>  
quit  

Example:

ft-echo> connect localhost 9000  
ft-echo> list  
ft-echo> put ../demo_data/test.txt  
ft-echo> get test.txt downloaded_test.txt  
ft-echo> quit  

---

### 3. FastAPI HTTP Wrapper

Linux / macOS:

cd server  
python3 fastapi_app.py  

Windows:

cd server  
python fastapi_app.py  

Using uvicorn:

uvicorn fastapi_app:app --host 0.0.0.0 --port 8000  

Endpoints:

| Method | Endpoint | Description |
|--------|---------|-------------|
| GET | /api/list | List files |
| GET | /api/get?file=<filename> | Download |
| POST | /api/put | Upload |
| POST | /api/resume | Resume |

---

### 4. React Frontend

cd frontend  
npm start  

Open:

http://localhost:3000  

Features:

- File list  
- Upload progress  
- Download with checksum  
- Auto refresh  

---

### 5. Automated Tests

Linux / macOS:

cd demo_scripts  
chmod +x run_tests.sh  
./run_tests.sh  

Windows:

cd demo_scripts  
run_tests.bat  

---

Simple Python Test:

Linux / macOS:

cd server  
python3 test_simple.py  

Windows:

cd server  
python test_simple.py  

---

## 🧪 Testing Instructions

PUT:

- Upload a file  
- Verify SHA256  
- Check storage  

GET:

- Download file  
- Compare checksum  
- Compare bytes  

RESUME:

- Interrupt upload  
- Resume  
- Verify checksum  

Manual tests:

demo_scripts/demo_commands.txt  

---

## 📤 Expected Output

- Upload success  
- Download success  
- Resume success  
- Logs in server/logs/server.log  

---

## ⚙ Implementation Details

Server:

- asyncio  
- Safe .part writes  
- SHA256 checks  
- Error handling  
- Logging  

Client:

- Sync API  
- Auto reconnect  
- FastAPI support  
- Checksum validation  

---

## ✅ Protocol Compliance

- 4-byte length prefix  
- 1-byte type  
- All message types  
- SHA256 integrity  
- Resume support  
- Chunk size = 4096  

---

## ⚙ Configuration

Environment:

FT_ECHO_HOST = localhost  
FT_ECHO_PORT = 9000  

Server settings:

DEFAULT_PORT  
CHUNK_SIZE  
STORAGE_DIR  

---

## 🛑 Troubleshooting

Server won’t start:

- Unix: lsof -i :9000  
- Windows: netstat -an | findstr 9000  
- Python: python3 --version  

Connection refused:

- Check server  
- Check firewall  
- Check host/port  

Checksum mismatch:

- Network stability  
- Chunk size  
- File integrity  

Resume broken:

- .part file exists  
- Correct offset  
- Same filename  

---

## 📄 License

MIT License

Copyright (c) 2026 Mukesh

Permission is hereby granted, free of charge, to any person obtaining a copy  
of this software and associated documentation files (the "Software"), to deal  
in the Software without restriction, including without limitation the rights  
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell  
copies of the Software, and to permit persons to whom the Software is  
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in  
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND.

---

## 👤 Author

Mukesh Ugrapalli  
FT-Echo Project – TCP File Transfer Implementation  
