# Use a imagem base do SDK do .NET para build e execução
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS runtime

# Definir diretório de trabalho
WORKDIR /app

# Copiar todos os arquivos da aplicação para o container
COPY . ./

# Restaurar dependências
RUN dotnet restore

# Compilar a aplicação
RUN dotnet build --no-restore -c Release

# Instalar a ferramenta dotnet-ef globalmente
RUN dotnet tool install --global dotnet-ef

# Adicionar o diretório de ferramentas globais ao PATH
ENV PATH="$PATH:/root/.dotnet/tools"

# Copie o script wait-for-it
COPY wait-for-it.sh .
RUN chmod +x wait-for-it.sh

# Aplicar as migrações no tempo de execução, garantindo que RabbitMQ e SQL Server estejam prontos
CMD ["./wait-for-it.sh", "rabbitmq:5672", "--", "./wait-for-it.sh", "sqlserver:1433", "--", "bash", "-c", "dotnet ef database update && dotnet run --no-build --urls http://0.0.0.0:5171"]