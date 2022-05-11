# 1 - CREATE YOUR APP

WebApi using .net sdk 3.1

# 2 - CREATE DOCKER FILE

#Open CMD:
1 - touch Dockerfile
2 - Add the content below in the dockerfile.

FROM mcr.microsoft.com/dotnet/core/sdk:3.1 AS build-env
WORKDIR /app
EXPOSE 5000
ENV ASPNETCORE_URLS=http://*:5000

# Copy csproj and restore as distinct layers
COPY *.csproj ./
RUN dotnet restore

# Copy everything else and build
COPY . ./
RUN dotnet publish -c Release -o out

# Build runtime image
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1
WORKDIR /app
COPY --from=build-env /app/out .
ENTRYPOINT ["dotnet", "{{YOUR_AP_NAME}}.dll"]

# 3 - CREATE DOCKER IGNORE FILE

#Open CMD:
1 - touch .dockerignore
2 - Add the content below in the dockerfile.

Dockerfile
[b|B]in
[O|o]bj

# 4 - CREATE YOUR IMAGE

docker build -t {{YOUR_AP_NAME}} .

# 5 - CREATE AND RUN YOUR CONTAINER

docker run -d -p 8080:80 --name {{YOUR_CONTAINER_NAME}} {{YOUR_AP_NAME}}

docker run -d -p 8080:80  --name dockerappready dockerapp