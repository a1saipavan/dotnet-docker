FROM mcr.microsoft.com/dotnet/sdk:6.0 AS base
WORKDIR /app

# copy csproj and restore as distinct layers
COPY *.sln .
COPY dotnetapp/*.csproj ./dotnetapp/
COPY utils/*.csproj ./utils/
COPY tests/*.csproj ./tests/
RUN dotnet restore "dotnetapp.csproj"

# copy and build everything else
COPY dotnetapp/. ./dotnetapp/
COPY utils/. ./utils/
COPY tests/. ./tests/

RUN dotnet build

FROM build AS testrunner
WORKDIR /app/tests
ENTRYPOINT ["dotnet", "test","--logger:trx"]

FROM build AS test
WORKDIR /app/tests
RUN dotnet test

FROM test AS publish
WORKDIR /app/dotnetapp
RUN dotnet publish -o out

FROM mcr.microsoft.com/dotnet:5.0-runtime AS runtime
WORKDIR /app
COPY --from=publish /app/dotnetapp/out ./
ENTRYPOINT ["dotnet", "dotnetapp.dll"]
