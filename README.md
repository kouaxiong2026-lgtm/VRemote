# VRemote v2 (WebRTC)
## 1) Server (VPS Ubuntu + domain)
    cd server && npm i ws
    TURN_HOST=turn.yourdomain.com TURN_SECRET=<random> node server.js     # port 8080
Put Caddy in front for HTTPS/WSS:  `yourdomain.com { reverse_proxy localhost:8080 }`
## 2) TURN (coturn) so it works behind any NAT/4G:  apt install coturn
    /etc/turnserver.conf:
      listening-port=3478
      use-auth-secret
      static-auth-secret=<same TURN_SECRET>
      realm=yourdomain.com
      min-port=49152
      max-port=65535
      no-multicast-peers
    Open firewall: 3478 tcp/udp, 49152-65535 udp
## 3) Build APK + Windows installer without installing anything
Push this folder to a GitHub repo -> Settings > Variables > Actions > add SERVER_URL = https://yourdomain.com
-> Actions tab -> run "build" -> download artifacts: VRemote-apk (app-debug.apk) and VRemote-Windows-Setup (VRemote-Setup.exe).
