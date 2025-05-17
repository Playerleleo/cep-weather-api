## Deploy no Google Cloud Run

A aplicação está disponível online através do Google Cloud Run:

### URL de Produção
https://cep-weather-api-bpv5e7fr7a-uc.a.run.app

Test:
https://cep-weather-api-bpv5e7fr7a-uc.a.run.app/?cep=01001000

curl "https://cep-weather-api-829419679442.us-central1.run.app/?cep=01001000"

### Exemplos de uso:

1. Consulta de CEP válido:
```
https://cep-weather-api-bpv5e7fr7a-uc.a.run.app/?cep=01001000
```

2. Health Check:
```
https://cep-weather-api-bpv5e7fr7a-uc.a.run.app/health
```

### Configuração no Google Cloud Run

Para o funcionamento correto da API no Cloud Run, é necessário configurar as seguintes variáveis de ambiente:

```
WEATHER_API_KEY=26de2d9ed37a4058b0c200241251605
```

Alternativamente, se você quiser usar o modo de teste (sem chamar a API do Weather):

```
TEST_MODE=true
```

Para mais informações sobre como fazer seu próprio deploy, veja o arquivo `DEPLOY.md`.

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

