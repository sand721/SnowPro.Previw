# Stage 1 � Build
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
# ������� ����� ��� ��������� �������
RUN mkdir -p /packages

# �������� ��������� NuGet ������
COPY ./nuget-packages/*.nupkg /packages/
#RUN ls -la /packages && exit 1

# ��������� ��������� ����� ��� �������� NuGet
RUN dotnet nuget add source /packages --name local-packages

WORKDIR /src

ARG ROOT_DIR=ProfileService
ARG PROJECT_NAME=ProfileService.API
ARG PROJECT_PATH=$ROOT_DIR/$PROJECT_NAME/$PROJECT_NAME.csproj

# �������� ���������
COPY $ROOT_DIR/. $ROOT_DIR/.

# �������������� ������������
RUN dotnet restore $PROJECT_PATH -p:USE_DOCKER_NUGET=true  -v detailed

# ������ � ����������
RUN dotnet build $PROJECT_PATH -c Release -o /app/build
RUN dotnet publish $PROJECT_PATH -c Release -o /app/publish

# Stage 2 � Runtime
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS final
WORKDIR /app
COPY --from=build /app/publish .

ENTRYPOINT ["dotnet", "ProfileService.API.dll"]
