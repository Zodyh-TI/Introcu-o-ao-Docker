# Introdução ao Docker

## 1. Começando o trabalho

Para começar o trabalho, foi criada uma máquina virtual utilizando o **Lubuntu**.

A ideia inicial era também configurar o acesso remoto utilizando SSH, porém essa parte não pôde ser realizada por causa dos **bloqueios e restrições de rede da instituição**. Por esse motivo, o trabalho foi continuado diretamente pela máquina virtual.

Depois da criação da máquina virtual, comecei preparando o sistema:

```terminal da maquina
sudo apt update
sudo apt upgrade -y
```

---

## 2. Instalando o Docker

Depois de atualizar o sistema, foi feita a instalação do Docker:

```terminal da maquina
sudo apt install docker.io -y
```

Depois conferi se a instalação tinha funcionado:

```terminal da maquina
docker --version
```

Também verifiquei o serviço do Docker:

```terminal da maquina
sudo systemctl status docker
```

Caso o serviço não estivesse iniciado, poderia ser iniciado com:

```terminal da maquina
sudo systemctl start docker
```

E para deixar o Docker iniciando automaticamente:

```terminal da maquina
sudo systemctl enable docker
```

---

## 3. Primeiro teste com Docker

Antes de criar a aplicação, fiz um teste simples para conferir se o Docker estava funcionando:

```terminal da maquina
sudo docker run hello-world
```

Esse comando baixa uma imagem de teste e executa um container.

Se a mensagem de confirmação aparecer no terminal, significa que o Docker está funcionando corretamente.

---

## 4. Criando a aplicação Flask

Depois disso, criei uma pasta para o projeto:

```terminal da maquina
mkdir projeto-flask
cd projeto-flask
```

Dentro da pasta, criei o arquivo `app.py`:

```terminal da maquina
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

Depois criei o arquivo `requirements.txt`:

```terminal da maquina
nano requirements.txt
```

E coloquei:

```text
Flask
```

Até aqui, a estrutura ficou:

```text
projeto-flask/
├── app.py
└── requirements.txt
```

---

## 5. Criando o Dockerfile

Agora criei o arquivo `Dockerfile`:

```terminal da maquina
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

A imagem `python:3.14-slim` foi utilizada porque ela é uma exigência do trabalho.

---

## 6. Criando a imagem Docker

Com os arquivos prontos, criei a imagem usando:

```terminal da maquina
sudo docker build -t projeto-flask .
```

Depois conferi as imagens disponíveis:

```terminal da maquina
sudo docker images
```

A imagem `projeto-flask` deve aparecer na lista.

---

## 7. Executando o container

Depois de criar a imagem, executei o container com:

```terminal da maquina
sudo docker run -d -p 5000:5000 --name projeto-flask-container projeto-flask
```

Para verificar se ele está funcionando:

```terminal da maquina
sudo docker ps
```

Se o container aparecer na lista, ele está rodando.

---

## 8. Testando a aplicação

Como a aplicação está usando a porta `5000`, podemos acessá-la pelo navegador através de:

```text
http://localhost:5000
```

A página deverá mostrar:

**Minha primeira aplicação Flask**

---

## 9. Alguns comandos utilizados

Durante a atividade, alguns comandos foram úteis para verificar e controlar o Docker.

### Ver os containers em execução

```terminal da maquina
sudo docker ps
```

### Ver todos os containers

```terminal da maquina
sudo docker ps -a
```

### Parar o container

```terminal da maquina
sudo docker stop projeto-flask-container
```

### Iniciar novamente

```terminal da maquina
sudo docker start projeto-flask-container
```

### Ver os logs

```terminal da maquina
sudo docker logs projeto-flask-container
```

### Ver as imagens

```terminal da maquina
sudo docker images
```

### Remover o container

```terminal da maquina
sudo docker rm projeto-flask-container
```

---

## 10. Resultado

Ao final dessa primeira etapa, a aplicação Flask está funcionando dentro de um container Docker.

A estrutura do projeto ficou:

```text
projeto-flask/
├── app.py
├── requirements.txt
└── Dockerfile
```

A etapa de acesso remoto por SSH não foi realizada devido aos **bloqueios de rede da instituição**. Por isso, as configurações e comandos do Docker foram realizados diretamente dentro da máquina virtual.

A partir dessa primeira aplicação, o projeto poderá continuar com a criação de **novas páginas, layouts diferentes e títulos distintos**.

## Tecnologias utilizadas

- Lubuntu
- Docker
- Python
- Flask
- `python:3.14-slim`

## Referências

- Repositório do GitHub do professor
