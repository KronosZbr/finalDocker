# Turorial

## Passo 1: Preparando o ambiente

1. Certifique-se de ter o Docker e o Docker Compose instalados na sua máquina.

## Passo 2: Preparar o ambiente Docker

### 2.1 Criar Diretório do Projeto

1. Crie um diretório para o seu projeto e navegue até ele:
   ```bash
   mkdir projdocker
   cd projdocker
   ```

### 2.2 Clone o Projeto

1. Clone o projeto dentro do diretório criado anteriormente:
   ```bash
   git clone "endereco do repositorio"
   ```

## Passo 3: Iniciar o projeto

1. Acesse o diretório do projeto que foi clonado:
   ```bash
   cd "diretorio clonado"
   ```
2. Após acessar o diretório, inicie os containers e aguarde o fim do processo:
   ```bash
   docker-compose up -d
   ```
3. Após o fim do processo, verifique se os containers estão rodando:
   ```bash
   docker ps
   ```

## Passo 4: Configurar o WordPress

### 4.1 Acessar o WordPress

1. Abra o navegador e acesse `http://localhost:8000` para completar a configuração do WordPress. Siga as instruções na tela para configurar o seu site WordPress.

### 4.2 Instalar o Plugin Redis Object Cache

1. No painel de administração do WordPress, vá para `Plugins > Adicionar Novo`.
2. Na barra de busca, digite "Redis Object Cache".
3. Encontre o plugin "Redis Object Cache" (desenvolvido por Till Krüss) e clique em `Instalar Agora`.
4. Após a instalação, clique em `Ativar`.

### 4.3 Editar o Arquivo `wp-config.php`

#### Acessar o Arquivo `wp-config.php`

1. Acesse o contêiner do WordPress:
    ```bash
    docker-compose exec wordpress bash
    ```

2. Navegue até o diretório do WordPress:
    ```bash
    cd /var/www/html
    ```
3. Instale o editor de código nano:
    ```bash
    apt-get update
    apt-get install nano
    ```

4. Edite o arquivo `wp-config.php` usando um editor de texto, como `nano`:
    ```bash
    nano wp-config.php
    ```

5. Adicione as seguintes linhas ao arquivo, logo acima da linha que diz `/* That's all, stop editing! Happy publishing. */`:

    ```php
    // Enable Redis Object Cache
    define('WP_CACHE', true);
    define('WP_REDIS_HOST', 'redis');
    define('WP_REDIS_PORT', 6379);
    ```

5. Salve e feche o arquivo:
   Pressione `CTRL+X`, depois `Y` e `ENTER` para salvar e sair.

### 4.4 Verificar a Configuração do Redis

1. No painel de administração do WordPress, vá para `Configurações > Redis`.
2. Na página de configurações do Redis, clique em `Enable Object Cache` (Ativar Cache de Objetos).
3. Verifique se o status está como conectado, isso indica que o Redis está conectado e funcionando.

## Passo 5: Verificar a Conexão do WordPress com o MySQL

### 5.1 Verificar a Instalação do WordPress

Se você conseguiu completar a instalação inicial do WordPress e acessar o painel de administração, a conexão com o MySQL está funcionando corretamente.

### 5.2 Verificar o Status do Banco de Dados no WordPress

1. Faça login no painel de administração do WordPress.
2. Instale e ative um plugin como o **Diagnóstico e resolução de problemas**.
3. Vá para `Ferramentas > Diagnóstico`.
4. Verifique se há algum erro relacionado ao banco de dados na seção `Status`.

### 5.3 Usar um Plugin de Administração de Banco de Dados

1. No painel de administração do WordPress, vá para `Plugins > Adicionar Novo`.
2. Pesquise por "WP phpMyAdmin" e instale o plugin.
3. Ative o plugin e vá para `Ferramentas > WP phpMyAdmin`.
4. Clique no botão `Enter local phpMyAdmin`.
5. Verifique se você consegue acessar e visualizar as tabelas do banco de dados.

### 5.4 Testar a Conexão Diretamente no Contêiner do WordPress

1. Acesse o contêiner do WordPress:
    ```bash
    docker-compose exec wordpress bash
    ```

2. Instale o `mysql-client` no contêiner do WordPress (se não estiver instalado):
    ```bash
    apt-get update
    apt-get install -y default-mysql-client
    ```

3. Conecte-se ao banco de dados MySQL:
    ```bash
    mysql -h db -u wpuser -ppassword wordpress
    ```

4. Execute uma consulta simples:
    ```sql
    SHOW TABLES;
    ```

    Se você ver uma lista de tabelas, a conexão está funcionando corretamente.

### 5.5 Verificar Logs de Erros

1. Verifique os logs do contêiner do WordPress:
    ```bash
    docker-compose logs wordpress
    ```

    Procure por mensagens de erro relacionadas ao banco de dados.

## Passo 6: Verificar o Prometheus

### 6.1 Acessar o Prometheus

1. Abra o navegador e acesse `http://localhost:9090` para verificar o Prometheus e suas métricas.
