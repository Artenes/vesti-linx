# Integração Vesti-Linx
API REST que permitirá a comunicação entre Vesti e o banco de dados do Linx.

# Instalação

    git clone https://github.com/Artenes/vesti-linx
    
    composer install --no-dev
    
    cp .env.example .env
    
# Configuração
    
## Banco de dados

No arquivo `.env` configure as credenciais de acesso ao banco de dados do Linx e do Vesti.

    VESTI_CONNECTION=mysql
    VESTI_HOST=127.0.0.1
    VESTI_PORT=3306
    VESTI_DATABASE=vesti
    VESTI_USERNAME=root
    VESTI_PASSWORD=secret
    
    LINX_CONNECTION=sqlsrv
    LINX_HOST=127.0.0.1
    LINX_PORT=1433
    LINX_DATABASE=linx
    LINX_USERNAME=root
    LINX_PASSWORD=secret
    
## Chave

    php artisan key:generate
    
## Cache

    php artisan config:cache
    
    php artisan route:cache

## Cron job

    * * * * * php /diretorio-do-projeto/artisan schedule:run >> /dev/null 2>&1

# Endpoints

## `GET /v1/product/{griffe}/{reference}`

### Parâmetros

- griffe: o nome da marca (ex. gangster)
- reference: o código de referência do produto (ex. 127454.3667)

### Resposta

    {
    
      response: {
		    status: 200,
		    message: "Produto encontrado"
	    },
      
	    data: {
		    reference: "1234.5567",
		    name: "Camiseta Laika Vermelha P",
		    composition: {},
		    sizes: ["P", "M", "GG"],
		    price: 12.80
	    }
    
    }
    
# Task

## Sincronização de preços

Uma vez ao dia, a aplicação irá realizar a sincronização entre os preços dos produtos do Linx e do Vesti.

É executado os seguintes passos:

- É listado todos os produtos que estão no banco de dados do Vesti (independente de marca)
- Cada produto é pesquisado no banco de dados do Linx
- O preço do produto encontrado no Linx é atualizado no produto do Vesti

Toda a atividade executada por essa task é registrada no log da aplicação `storage/logs/laravel.log`.
