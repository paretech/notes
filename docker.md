# Docker Quick Reference

## Running Interactive Containers

### Basic Interactive Container Command
```bash
docker run --rm -it --name mytemp python:3.12-slim bash
```

**What this does (in order):**
- `docker run` - Creates and starts a new container from an image
- `--rm` - **Auto-remove** - Deletes the container automatically when it exits (no leftover stopped containers)
- `-i` - **Interactive** - Keeps STDIN open even if not attached (lets you type input)
- `-t` - **TTY** - Allocates a pseudo-terminal (gives you a proper terminal interface)
- `--name mytemp` - **Container name** - Names this container "mytemp" (easier to reference than random ID)
- `python:3.12-slim` - The image to use (explained below)
- `bash` - Command to run inside the container (starts a bash shell)

**Why this combination matters:**
- **`-it` together:** Nearly always used as a pair. `-i` lets you interact, `-t` makes it look like a real terminal with prompt, colors, etc.
- **`--rm`:** Clean up after yourself - container disappears when you exit. Without this, you get stopped containers piling up (visible with `docker ps -a`)
- **`--name mytemp`:** Lets you use `docker exec -it mytemp bash` or `docker stop mytemp` instead of looking up container IDs. Note: name must be unique - you can't run two containers with the same name simultaneously.

### From Interactive Session to Dockerfile (Trust but Verify Workflow)

Now that you know how to create an interactive session, you probably want to start capturing your work to a Dockerfile so it's reproducible. Here's the industry-standard iterative workflow:

**Phase 1: Interactive exploration** (figure out what actually works)
```bash
# Start with volume mount so you can access your files
docker run --rm -it --name mytemp \
  -v $(pwd):/workspace \
  -w /workspace \
  python:3.12-slim bash

# Inside container, manually test commands:
apt-get update
apt-get install -y some-package
pip install pandas numpy
python test_script.py
# Take notes of what works!
```

**Phase 2: Capture to Dockerfile incrementally**
Create a `Dockerfile` in your project directory and add commands as you verify them:
```dockerfile
FROM python:3.12-slim

# Add system packages (clean up apt cache to keep image small)
RUN apt-get update && apt-get install -y \
    some-package \
    another-package \
 && rm -rf /var/lib/apt/lists/*

# Copy and install Python dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Set working directory
WORKDIR /app

# Copy your code
COPY . .

# Optional: Define what runs when container starts
CMD ["python", "app.py"]
```

**Phase 3: Build and test the Dockerfile**
```bash
# Build image (tag it for easy reference)
docker build -t myapp:dev .

# Test it works like your interactive session
docker run --rm -it myapp:dev bash

# Or run your app directly
docker run --rm myapp:dev
```

**Phase 4: Iterate - Edit → Build → Test**
```bash
# Edit Dockerfile, then rebuild
docker build -t myapp:dev .

# Docker caches layers! Only rebuilds from changed line down
# This makes iteration fast

# Test again
docker run --rm -it myapp:dev bash
```

**Pro tips for this workflow:**
- **Layer caching:** Docker caches each RUN/COPY command. Put things that change rarely (system packages) early, things that change often (your code) late.
- **See build details:** `docker build --progress=plain -t myapp:dev .` shows exactly what each command does
- **Debug builds:** If build fails, you can inspect the last successful layer to debug
- **Volume mount during development:** Even with a Dockerfile, you can still mount code to test changes without rebuilding:
  ```bash
  docker run --rm -it -v $(pwd):/app myapp:dev bash
  ```

**Why this workflow works:**
- ✅ Interactive exploration lets you verify before committing to Dockerfile
- ✅ Dockerfile becomes your documented, reproducible recipe
- ✅ Layer caching makes rebuilds fast (only changed parts rebuild)
- ✅ You can always drop into interactive mode to debug
- ✅ Final Dockerfile can be versioned in git and shared with team

### Python Slim Image Explained

**Image format:** `python:3.12-slim`
- `python` - Base image name (official Python image)
- `3.12` - Python version (can use 3.11, 3.10, etc.)
- `slim` - Image variant

**Slim vs Standard vs Alpine:**

| Variant | Size | Description | Use When |
|---------|------|-------------|----------|
| `python:3.12` | ~900MB | Full Debian-based image with build tools, libraries | You need C compilers, system libraries, or debugging tools |
| `python:3.12-slim` | ~120MB | Minimal Debian with just Python runtime | **Best default choice** - Production apps, most dependencies work |
| `python:3.12-alpine` | ~50MB | Minimal Alpine Linux based | Size critical AND you don't need C extensions (can break packages like numpy, pandas) |

**Why slim is usually best:**
- Much smaller than standard (saves disk/bandwidth)
- More compatible than alpine (doesn't break C-based Python packages)
- Still has package manager (apt) if you need to install system dependencies

**Check image size:**
```bash
# See size of all your images
docker images

# See size of specific image
docker images python:3.12-slim
```
The SIZE column shows the compressed image size on disk.

## Essential Commands Quick Reference

### Container Lifecycle
```bash
# Run container (one-off command)
docker run IMAGE COMMAND

# Run container in background (detached)
docker run -d IMAGE

# Run interactive container with TTY
docker run -it IMAGE bash

# Run with name
docker run --name my-container IMAGE

# Remove container after exit
docker run --rm IMAGE
```

### Volume Mounting (Access Host Files)
```bash
# Mount current directory to /app in container
docker run -v $(pwd):/app IMAGE

# Mount with read-only
docker run -v $(pwd):/app:ro IMAGE

# Named volume (Docker manages storage)
docker run -v my-data:/data IMAGE
```

### Port Mapping (Expose Services)
```bash
# Map host port 8080 to container port 80
docker run -p 8080:80 IMAGE

# Map all exposed ports to random host ports
docker run -P IMAGE
```

### Environment Variables
```bash
# Set single variable
docker run -e MY_VAR=value IMAGE

# Load from file
docker run --env-file .env IMAGE
```

### Container Management
```bash
# List running containers
docker ps

# List all containers (including stopped)
docker ps -a

# Stop container
docker stop CONTAINER_ID

# Start stopped container
docker start CONTAINER_ID

# Remove container
docker rm CONTAINER_ID

# Remove all stopped containers
docker container prune

# Execute command in running container
docker exec -it CONTAINER_ID bash

# View logs
docker logs CONTAINER_ID

# Follow logs (like tail -f)
docker logs -f CONTAINER_ID
```

### Image Management
```bash
# List images
docker images

# Pull image from registry
docker pull IMAGE

# Remove image
docker rmi IMAGE

# Remove unused images
docker image prune

# Build image from Dockerfile
docker build -t my-image:tag .

# Tag image
docker tag SOURCE_IMAGE TARGET_IMAGE
```

## Common Patterns

### Development Container with Code Mount
```bash
# Mount your code, run interactively, remove when done
docker run -it --rm -v $(pwd):/app -w /app python:3.11-slim bash
```
- `-w /app` sets working directory inside container
- Changes to files persist on host (volume mounted)
- Container deleted on exit (--rm)

### Quick Python Script Test
```bash
# Run script without installing Python locally
docker run --rm -v $(pwd):/app -w /app python:3.11-slim python script.py
```

### Web Server with Port Forward
```bash
# Run Flask/Django app accessible at localhost:5000
docker run -p 5000:5000 -v $(pwd):/app -w /app python:3.11-slim python app.py
```

## Key Concepts (Fast Review)

**Images vs Containers:**
- **Image** = Blueprint (immutable, like a class)
- **Container** = Running instance (like an object)
- One image can create many containers

**Container Isolation:**
- Own filesystem (starts fresh from image each time unless volumes used)
- Own process space
- Own network interface (unless host network mode)
- Does NOT persist changes unless volumes/commits used

**Volumes (Data Persistence):**
- Bind mounts: `-v /host/path:/container/path` - Use specific host directory
- Named volumes: `-v volume-name:/container/path` - Docker manages location
- Anonymous volumes: `-v /container/path` - Temporary, Docker manages

**Port Mapping:**
- Containers have own network namespace
- Ports inside container isolated from host
- `-p HOST_PORT:CONTAINER_PORT` creates tunnel
- Example: `-p 3000:80` means localhost:3000 � container's port 80

## Troubleshooting Quick Fixes

```bash
# Container exits immediately?
# - Make sure command keeps running (bash exits if not interactive)
docker run -it IMAGE bash  # Need -it for interactive bash

# Can't access files?
# - Check volume mount path
docker run -v $(pwd):/app -w /app IMAGE ls /app

# Changes not persisting?
# - Need volume mount, container filesystem is ephemeral

# Port not accessible?
# - Check port mapping, check app binds to 0.0.0.0 not 127.0.0.1
docker run -p 8080:8080 IMAGE

# Permission issues with volumes?
# - Container user may differ from host user
docker run -u $(id -u):$(id -g) -v $(pwd):/app IMAGE
```

## Cleanup Commands

```bash
# Nuclear option - remove everything not in use
docker system prune -a

# Remove all stopped containers
docker container prune

# Remove dangling images (untagged)
docker image prune

# Remove unused volumes
docker volume prune
```
