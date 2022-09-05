FROM mcr.microsoft.com/dotnet/sdk:6.0 AS base
WORKDIR /app

# copy csproj and restore as distinct layers
COPY *.sln .
COPY dotnetapp/*.csproj ./dotnetapp/
COPY utils/*.csproj ./utils/
COPY tests/*.csproj ./tests/
RUN dotnet restore

# copy and build everything else
COPY dotnetapp/. ./dotnetapp/
COPY utils/. ./utils/
COPY tests/. ./tests/

RUN dotnet build

#FROM base AS testrunner
#WORKDIR /app/tests
#ENTRYPOINT ["dotnet", "test","--logger:trx"]

#FROM testrunner AS test
#WORKDIR /app/tests
#RUN dotnet test

FROM base AS publish
WORKDIR /app/dotnetapp
RUN dotnet publish -o out

#mcr.microsoft.com/dotnet:5.0-runtime

FROM mcr.microsoft.com/dotnet/runtime:6.0 AS runtime
WORKDIR /app
COPY --from=publish /app/dotnetapp/out ./
ENTRYPOINT ["dotnet", "dotnetapp.dll"]
