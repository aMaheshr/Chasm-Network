<h1 align="center">  Chasm Network Node Setup

## Basic Steps 

## minimum System Requirements
| Ram | cpu     | disk                      |
| :-------- | :------- | :-------------------------------- |
| `1 GB`      | `1 Core` | `5-20 GB SSD` |

## Step 1: Mint NFT to Get Scout ID and API Key 
- Visit https://scout.chasm.net/private-mint

Mint NFT : 0.3 $MNT

![zz](https://github.com/user-attachments/assets/5eeee414-875e-4061-8d89-884b2a2dc7f3)


- Click setup my Scouts : Get ur `WEBHOOK_API_KEY` and `SCOUT_UID` 

## Step 2: Get Groq API Key

Sign up for an account at [Groq](https://console.groq.com/keys) to `get GROQ_API_KEY`

![zzz](https://github.com/user-attachments/assets/e5f07363-8e26-4651-8a11-9624ceec4199)



# Lets Start Installation

##  Install Dependecies

```console
# Install Docker

sudo apt-get update
sudo apt-get install ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add repository to Apt sources:

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update

# Docker

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

## Setup

```console
cd ~
```

### Create and config .env file
```console
nano .env
```

- `SCOUT_NAME`: Your Favorite scout name
- `SCOUT_UID`: Enter ur NFT UID
- `WEBHOOK_API_KEY`: Enter ur API_KEY 
- `WEBHOOK_URL`: Replace ur IP : http://"Your IP":3001/
- `GROQ_API_KEY`: Enter ur Groq API Key

  
```console
PORT=3001
LOGGER_LEVEL=debug

# Chasm
ORCHESTRATOR_URL=https://orchestrator.chasm.net
SCOUT_NAME=ur name
SCOUT_UID=
WEBHOOK_API_KEY=
WEBHOOK_URL=

# Chosen Provider (groq, openai)
PROVIDERS=groq
MODEL=gemma2-9b-it
GROQ_API_KEY=

# Optional Leave it 
OPENROUTER_API_KEY=
OPENAI_API_KEY=

NODE_ENV=production
```
save .env file and exit: `Ctrl + X`  `Y` , `Enter`

### Run

```console
# Open Port
sudo ufw allow 3001

# Pull code from DockerHub
docker pull johnsonchasm/chasm-scout

# Start docker file
docker run -d --restart=always --env-file ./.env -p 3001:3001 --name scout johnsonchasm/chasm-scout
```

### Verify Server

```console
# Responce "OK" Ur Good to go
curl localhost:3001
```

### LLM

```console
source ./.env
curl -X POST \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer $WEBHOOK_API_KEY" \
     -d '{"body":"{\"model\":\"gemma-7b-it\",\"messages\":[{\"role\":\"system\",\"content\":\"You are a helpful assistant.\"}]}"}' \
     $WEBHOOK_URL
```

### Check Logs

```console
docker logs scout
```

### u can Check leaderboard
https://scout.chasm.net/leaderboard

--------------------------------------------------------------------------------------------------------------------------------------------------

## update to latest version 

- stop Docker
```console
docker stop scout && docker rm scout
```

```console
docker pull johnsonchasm/chasm-scout:latest
```
```console
docker run -d --restart=always --env-file ./.env -p 3001:3001 --name scout johnsonchasm/chasm-scout
```

--------------------------------------------------------------------------------------------------------------------------------

## optional :
### Kill and stop docker
```console
docker stop scout && docker rm scout
```


###  Restart Docker
```console
docker restart scout
```

Thats it
Follow on  X : https://x.com/CryptoCrocks
