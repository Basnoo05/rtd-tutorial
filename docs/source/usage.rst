Usage
=====

Setup
1. Clone the Repo
```bash
git clone https://github.com/studbudz/studbudz.git
cd studbudz
flutter pub get
```

2. Get Your IP Address
```bash
Windows: ipconfig → Look for IPv4 Address
macOS: ifconfig → Look for inet under en0
Linux: hostname -I
```
3. Update Config
Edit config.dart with your IP:
```
const String apiUrl = 'http://192.168.X.X:8080'; // Replace with your IP
```
4. Run the App
    1. Create 2 terminals
    2. cd server -> dart run
    3. cd studbudz -> flutter run

flutter run
✅ Troubleshooting

Ensure device and server are on the same network.
Check firewall settings for port 8080.
