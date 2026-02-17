# Docker Run Command Guide (MkDocs Example)

This guide explains the meaning of the following Docker command and how image names like `mkdocs-dev` and `my-mkdocs` affect behavior.

```bash
docker run --rm -it -p 8000:8000 -v $(pwd):/mkdocs mkdocs-dev
```

---

## 1. Basic Structure of docker run

General syntax:

```bash
docker run [OPTIONS] IMAGE_NAME [COMMAND]
```

Example:

```bash
docker run --rm -it -p 8000:8000 -v $(pwd):/mkdocs mkdocs-dev
```

Components:

| Part       | Meaning                      |
| ---------- | ---------------------------- |
| docker run | Start a new container        |
| OPTIONS    | Configure container behavior |
| IMAGE_NAME | The environment to run       |
| COMMAND    | Optional override command    |

---

## 2. Explanation of Each Option

## --rm

```bash
--rm
```

Meaning:

* Automatically deletes the container after it stops

Why useful:

* Keeps system clean
* Avoids unused container buildup

Without `--rm`:

Container remains after exit.

With `--rm`:

Container is deleted automatically.

---

## -it

```bash
-it
```

This is combination of:

```bash
-i   (interactive)
-t   (terminal)
```

Meaning:

* Allows interactive use
* Shows logs and output in your terminal
* Lets you press Ctrl+C to stop

Required for development servers like MkDocs.

---

## -p 8000:8000

```bash
-p HOST_PORT:CONTAINER_PORT
```

Example:

```bash
-p 8000:8000
```

Meaning:

* Maps port from container → host machine

Structure:

```
host:container
```

Result:

```
Browser → localhost:8000 → Docker container port 8000
```

This allows you to open MkDocs in your browser:

```
http://localhost:8000
```

---

## -v $(pwd):/mkdocs

```bash
-v HOST_FOLDER:CONTAINER_FOLDER
```

Example:

```bash
-v $(pwd):/mkdocs
```

Meaning:

Mounts your current directory into the container.

Breakdown:

| Part    | Meaning                     |
| ------- | --------------------------- |
| $(pwd)  | Your current folder on host |
| /mkdocs | Folder inside container     |

Result:

```
Your files → available inside container
```

Example:

Host:

```
/home/user/docs
```

Container:

```
/mkdocs
```

MkDocs reads files from `/mkdocs`.

Changes update instantly.

---

## 3. The Image Name (IMPORTANT PART)

Final part of command:

```bash
mkdocs-dev
```

This is the Docker image name.

Docker images are like:

* Templates
* Environments
* Preconfigured systems

They contain:

* OS
* Python
* MkDocs
* plugins
* themes

---

## 4. Example: mkdocs-dev vs my-mkdocs

Two commands:

```bash
docker run --rm -it -p 8000:8000 -v $(pwd):/mkdocs mkdocs-dev
```

```bash
docker run --rm -it -p 8000:8000 -v $(pwd):/mkdocs my-mkdocs
```

Difference:

Only the image name:

| Image      | Meaning                |
| ---------- | ---------------------- |
| mkdocs-dev | Image named mkdocs-dev |
| my-mkdocs  | Image named my-mkdocs  |

They may contain different:

* MkDocs version
* plugins
* themes
* Python version

Example:

mkdocs-dev:

```
MkDocs only
```

my-mkdocs:

```
MkDocs + Material theme + plugins
```

---

## 5. Visual Diagram

```
Your Computer

/docs folder
   │
   │ mounted via -v
   ▼
Docker Container

/mkdocs folder
   │
   │ served by MkDocs
   ▼
Port 8000 inside container
   │
   │ mapped via -p
   ▼
localhost:8000 on your computer
```

---

## 6. Real Example Workflow

Step 1: Build image

```bash
docker build -t my-mkdocs .
```

Step 2: Run server

```bash
docker run --rm -it -p 8000:8000 -v $(pwd):/mkdocs my-mkdocs
```

Step 3: Open browser

```
http://localhost:8000
```

---

## 7. Summary Table

| Part                   | Meaning                     |
| ---------------------- | --------------------------- |
| docker run             | Start container             |
| --rm                   | Delete container after stop |
| -it                    | Interactive terminal        |
| -p 8000:8000           | Map ports                   |
| -v $(pwd):/mkdocs      | Mount folder                |
| mkdocs-dev / my-mkdocs | Image name                  |

---

## 8. Key Concept Summary

Docker image = Environment

Docker container = Running instance of image

Your command means:

"Run MkDocs inside Docker, use my current folder, and show it on localhost:8000"

---

## 9. Check available images

```bash
docker images
```

Example output:

```
REPOSITORY   TAG
mkdocs-dev   latest
my-mkdocs    latest
```

---

## 10. Stop container

Press:

```
Ctrl + C
```

Container is removed automatically because of `--rm`.

---

## End of Guide
