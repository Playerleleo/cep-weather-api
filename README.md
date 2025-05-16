# API de Clima por CEP

Esta API recebe um CEP brasileiro e retorna a temperatura atual da localidade em Celsius, Fahrenheit e Kelvin.

## Requisitos

- Go 1.21 ou superior
- Docker e Docker Compose
- Chave de API do WeatherAPI (https://www.weatherapi.com/)

## Configuração

1. Clone o repositório
2. Copie o arquivo `.env.example` para `.env`
3. Adicione sua chave de API do WeatherAPI no arquivo `.env`

## Executando localmente

```bash
go run main.go
```

## Executando com Docker

```bash
docker-compose up --build
```

## Uso

Faça uma requisição GET para a API com o CEP como parâmetro:

```
GET http://localhost:8080/?cep=12345678
```

### Respostas

#### Sucesso (200)
```json
{
    "temp_C": 28.5,
    "temp_F": 83.3,
    "temp_K": 301.5
}
```

#### CEP Inválido (422)
```
invalid zipcode
```

#### CEP Não Encontrado (404)
```
can not find zipcode
```

## Health Check

O endpoint de verificação de saúde pode ser acessado em:

```
GET http://localhost:8080/health
```

Resposta esperada:
```json
{
    "status": "ok"
}
```

## Testes

Para executar a aplicação em modo de teste (sem chamar APIs externas):

```bash
# Via variável de ambiente
TEST_MODE=true go run main.go

# Ou com Docker
docker-compose -f docker-compose.test.yml up --build
```

## Deploy no Google Cloud Run

Para fazer o deploy no Google Cloud Run:

1. Instale e configure o Google Cloud SDK
2. Faça build da imagem Docker e envie para o Container Registry
3. Deploy no Cloud Run

```bash
# Build da imagem Docker
docker build -t gcr.io/[PROJECT_ID]/cep-weather-api .

# Enviar para o Container Registry
docker push gcr.io/[PROJECT_ID]/cep-weather-api

# Deploy no Cloud Run
gcloud run deploy cep-weather-api \
  --image gcr.io/[PROJECT_ID]/cep-weather-api \
  --platform managed \
  --region us-central1 \
  --allow-unauthenticated \
  --set-env-vars="WEATHER_API_KEY=[SUA_CHAVE_API]"
```