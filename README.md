# 🚀 Zyper-Panel v1

⚠️ **Note:**  
Zyper-Panel v1 is currently under development and may contain bugs.  
We are actively fixing issues — please report bugs in the **ZyperDevelopment Discord server**.

---

## 📥 Installation

Copy and run the following commands to install **Zyper-Panel v1**.

---

### 📂 Clone the Repository
```bash
git clone https://github.com/zyperdev1/Zyper-Panel-v1
```
📁 Go to the Project Directory
```
cd Zyper-Panel-v1
```

📦 Install Node.js / npm Dependencies
```
npm install
```

⚙️ Setup Environment Variables
```
cp example.env .env
```
🛠️ Build the Panel
```
npm run migrate:dev && npm run build && npm run seed
```
▶️ Start the Panel
```
npm run start
```
go to https://github.com/zyperdev1/daemon-1
for node installation
After Node installation Create A Minecraft server then
Stop the panel using command=
Ctrl + C 
Then go to
if server paper then= daemon/servers/minecraft/serverid/start.sh 
if server minecraft then= daemon/servers/generic/serverid/start.sh
if server is node.js or python go to the path's like that.
it is currently only for Paper so, later on you can run different server currently only for paper
edit the start.sh
```
#!/bin/bash
set -e

# =====================================
# Configuration (Panel Variables)
# =====================================
SERVER_DIR="/project/workspace/daemon/zyperpanel-daemon/servers/minecraft/6428f01e-ff2a-4acb-bd98-3fd6b26dcb70"

export PORT={port}
export VERSION="{server-version}"

# BUILD OPTIONS:
#   empty  -> latest build
#   number -> specific Paper build (e.g. 450)
export PAPER_BUILD=""  

export EULA="TRUE"

# =====================================
# Bootstrap
# =====================================
cd "$SERVER_DIR" || exit 1

echo "Starting ZyperPanel Minecraft Server"
echo "Version: ${VERSION}"

# =====================================
# Server Properties
# =====================================
cat > server.properties << EOF
server-port=${PORT}
max-players=20
view-distance=10
online-mode=false
level-name=world
EOF

# =====================================
# Ensure jq exists
# =====================================
if ! command -v jq &>/dev/null; then
  echo "ERROR: jq is required but not installed"
  exit 1
fi

# =====================================
# Fetch ALL Paper Builds
# =====================================
echo "Fetching PaperMC build list for ${VERSION}..."

BUILD_LIST_JSON=$(curl -fsSL \
  "https://api.papermc.io/v2/projects/paper/versions/${VERSION}")

ALL_BUILDS=$(echo "$BUILD_LIST_JSON" | jq -r '.builds[]')

LATEST_BUILD=$(echo "$BUILD_LIST_JSON" | jq -r '.builds[-1]')

# =====================================
# Select Build
# =====================================
if [ -z "$PAPER_BUILD" ]; then
  BUILD="$LATEST_BUILD"
  echo "Using latest Paper build: ${BUILD}"
else
  if echo "$ALL_BUILDS" | grep -qx "$PAPER_BUILD"; then
    BUILD="$PAPER_BUILD"
    echo "Using selected Paper build: ${BUILD}"
  else
    echo "ERROR: Paper build ${PAPER_BUILD} does not exist for ${VERSION}"
    echo "Available builds:"
    echo "$ALL_BUILDS"
    exit 1
  fi
fi

# =====================================
# Download Server JAR
# =====================================
JAR_NAME="paper-${VERSION}-${BUILD}.jar"
DOWNLOAD_URL="https://api.papermc.io/v2/projects/paper/versions/${VERSION}/builds/${BUILD}/downloads/${JAR_NAME}"

if [ ! -f server.jar ]; then
  echo "Downloading ${JAR_NAME}..."
  curl -fsSL -o server.jar "$DOWNLOAD_URL"
fi

# =====================================
# Validate JAR
# =====================================
if ! file server.jar | grep -q "Java archive"; then
  echo "ERROR: server.jar is invalid"
  ls -lh server.jar
  exit 1
fi

SIZE_MB=$(du -m server.jar | cut -f1)
if [ "$SIZE_MB" -lt 10 ]; then
  echo "ERROR: server.jar is too small (${SIZE_MB}MB)"
  exit 1
fi

# =====================================
# EULA
# =====================================
echo "eula=true" > eula.txt

# =====================================
# Start Server
# =====================================
echo "Launching Minecraft Paper ${VERSION} (build ${BUILD})..."
exec java \
  -Xms512M \
  -Xmx1024M \
  -jar server.jar \
  nogui < /dev/tty
```
replace the {port} and the {server-version} with the server port and version you have created in the panel!
the start the panel again=
```
npm start
```
And now you can manage the server start, stop, restart, everything!
💬 Join Our Discord Community
<p align="center">
  <a href="https://discord.gg/hDb8mSHt8E">
    <img src="https://avatars.githubusercontent.com/u/195574965?v=4" width="180" alt="ZyperDevelopment Discord">
  </a>
</p>
