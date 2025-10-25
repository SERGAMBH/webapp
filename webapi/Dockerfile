# укажем, на основе какого образа будем 
# делать наш контейнер. Для сборки приложения используем dotnet-sdk 
# и назовём этот образ builder
FROM mcr.microsoft.com/dotnet/sdk:latest AS builder 
EXPOSE 8080
# укажем директорию для нашего приложения внутри контейнера
WORKDIR /Application
# Скопируем все файлы из проекта в файловую систему контейнера
COPY . ./
# Запустим restore для загрузки зависимостей
COPY nuget-packages/ /nuget-packages/
RUN dotnet restore "webapi.csproj" --source /nuget-packages --disable-parallel
# Опубликуем собранный dll в папку "output"
RUN dotnet publish -c Release -o output
# Теперь соберём образ, в котором наше приложение 
# будет запущено. Для запуска приложения достаточно
# среды выполнения aspnet, без sdk
FROM mcr.microsoft.com/dotnet/aspnet:latest
WORKDIR /Application
# Скопируем файлы приложения из предыдущего образа 
COPY --from=builder /Application/output .
# укажем команду, которая будет запускать приложение
ENTRYPOINT ["dotnet", "webapi.dll"]