# Introdução ao Docker

## 1. Começando o trabalho

Para começar o trabalho, primeiro foi criada uma máquina virtual usando o **Lubuntu**. Depois disso, foi configurado o acesso por SSH para conseguir acessar a máquina pelo terminal sem precisar ficar usando diretamente a máquina virtual.

Primeiro atualizei os pacotes:

```bash
sudo apt update
sudo apt upgrade -y
```

Depois instalei o SSH:

```bash
sudo apt install openssh-server -y
```

Para conferir se o SSH estava funcionando:

```bash
sudo systemctl status ssh
```

Se aparecer `active (running)`, significa que o serviço está funcionando.

Para descobrir o IP da máquina virtual usei:

```bash
hostname -I
```

Com o IP em mãos, é possível entrar na máquina pelo outro computador usando:

```bash
ssh usuario@IP_DA_MAQUINA
```

Por exemplo:

```bash
ssh aluno@192.168.1.100
```

---

## 2. Instalando o Docker

Depois de configurar o SSH, comecei a instalação do Docker.

Primeiro atualizei os pacotes:

```bash
sudo apt update
```

Depois instalei o Docker:

```bash
sudo apt install docker.io -y
```

Para conferir se foi instalado:

```bash
docker --version
```

Também verifiquei se o serviço estava funcionando:

```bash
sudo systemctl status docker
```

Caso o Docker não esteja iniciado, podemos iniciar com:

```bash
sudo systemctl start docker
```

E deixar configurado para iniciar junto com o sistema:

```bash
sudo systemctl enable docker
```

---

## 3. Primeiro teste com Docker

Antes de começar a aplicação, fiz um teste simples para verificar se o Docker estava funcionando:

```bash
sudo docker run hello-world
```

O Docker baixa uma imagem de teste e executa um container. Se aparecer a mensagem de confirmação, a instalação está funcionando.

---

## 4. Criando a aplicação Flask

Depois disso, criei uma pasta para guardar o projeto:

```bash
mkdir introducao-docker
cd introducao-docker
```

Dentro dela criei o arquivo `app.py`:

```bash
nano app.py
```

Coloquei o seguinte código:

```python
from flask import Flask

app = Flask(__name__)

@app.route("/")
def inicio():
    return "<h1>Minha primeira aplicação Flask</h1>"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

Salvei o arquivo e saí do `nano`.

Depois criei o `requirements.txt`:

```bash
nano requirements.txt
```

E coloquei:

```text
Flask
```

Nesse momento, a pasta ficou assim:

```text
introducao-docker/
├── app.py
└── requirements.txt
```

---

## 5. Criando o Dockerfile

Agora criei o arquivo que será usado pelo Docker para montar a imagem:

```bash
nano Dockerfile
```

Dentro dele coloquei:

```dockerfile
FROM python:3.14-slim

WORKDIR /app

COPY requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY app.py .

EXPOSE 5000

CMD ["python", "app.py"]
```

A imagem `python:3.14-slim` é obrigatória neste trabalho, por isso ela foi utilizada na primeira linha do Dockerfile.

---

## 6. Criando a imagem

Com os arquivos prontos, fiz a construção da imagem:

```bash
sudo docker build -t introducao-flask .
```

Depois conferi se ela tinha sido criada:

```bash
sudo docker images
```

A imagem `introducao-flask` deve aparecer na lista.

---

## 7. Executando o container

Agora podemos criar o container usando a imagem que acabamos de criar:

```bash
sudo docker run -d -p 5000:5000 --name introducao-flask-container introducao-flask
```

Para conferir se ele está rodando:

```bash
sudo docker ps
```

Se o container aparecer na lista, está funcionando.

---

## 8. Testando a aplicação

Para acessar a aplicação na própria máquina:

```text
http://localhost:5000
```

Se for acessar de outro computador da mesma rede, podemos usar o IP da máquina virtual:

```text
http://IP_DA_MAQUINA:5000
```

Por exemplo:

```text
http://192.168.1.100:5000
```

Ao acessar a página, deverá aparecer:

**Minha primeira aplicação Flask**

---

## 9. Alguns comandos que usei

Durante o trabalho, alguns comandos são úteis para controlar os containers.

Para ver os containers rodando:

```bash
sudo docker ps
```

Para ver todos os containers:

```bash
sudo docker ps -a
```

Para parar o container:

```bash
sudo docker stop introducao-flask-container
```

Para iniciar novamente:

```bash
sudo docker start introducao-flask-container
```

Para ver os logs:

```bash
sudo docker logs introducao-flask-container
```

Para ver as imagens:

```bash
sudo docker images
```

---

## 10. Resultado

Depois desses passos, a aplicação Flask já está funcionando dentro de um container Docker.

A estrutura do projeto ficou:

```text
introducao-docker/
├── app.py
├── requirements.txt
└── Dockerfile
```

Essa é a primeira parte do trabalho. A partir dela, a aplicação poderá ser modificada para receber **novas páginas, layouts diferentes e títulos diferentes**, além das outras partes que serão desenvolvidas no projeto.

**Tecnologias usadas até aqui:**

- Lubuntu
- SSH
- Docker
- Python
- Flask
- `python:3.14-slim`
