# DevOps Case Study - Webscraper with Selenium and CI/CD GitHub Pipeline


Github actions deployment file:
```
name: build, test and upload artifact

on:
  push:
  pull_request:
    branches: [ master ]
    paths:
    - 'DevOps_webscraper.sln'


env:
  DOTNET_VERSION: '5.0.301' # The .NET SDK version to use

jobs:
  build-and-test:

    name: build-and-test-${{matrix.os}}
    runs-on: ${{ matrix.os }}
    strategy:
      matrix:
        os: [windows-latest]

    steps:
    - uses: actions/checkout@v2
    - name: Setup .NET Core
      uses: actions/setup-dotnet@v1
      with:
        dotnet-version: ${{ env.DOTNET_VERSION }}

    - name: Install dependencies
      run: dotnet restore
      
    - name: Build
      run: dotnet build --configuration Release --no-restore
    
    - name: publish
      run: |
          dotnet publish
    
    - name: Test
      run: dotnet test --no-restore --verbosity normal
      
    - name: Upload a Build Artifact
      uses: actions/upload-artifact@v2.2.2
      with:
        # Artifact name
        name: Webscraper #.zip will be added automatically
        path: "./DevOps_webscraper/bin/Debug/net5.0/win-x64/publish/"
```
