# Confab Development

!!! info "This section of the documentation is intended for developers"

## Setting Up Dev Environment

### 1. Backend

1. Clone the main Confab repo (and submodules) with `git clone https://github.com/{{variables.CONFAB_GITHUB_LOCATION}}.git --recursive`
1. Initially compile email templates with `python3 Confab/email_designs/compile.py`
1. Open `Confab/Confab.sln` in Visual Studio
1. Open `appsettings.json` and set `ConfabParams:ExternalUrl` to "localhost", and enter an email address in `UserRoles:Admin`
1. Start the program. If everything worked correctly, Swagger UI should be available at `http://localhost:2632/swagger/index.html`


### 2. ConfabUI

1. `cd` to the ConfabUI submodule directory
1. Create a `.env` file in this directory and add the following
```
PUBLIC_API_URL="http://localhost:2632"
```
3. Start the dev server with `npm install && npm run dev`

### 3. First Login

1. Navigate to the frontend dev server URL (Default is `http://localhost:5173/`)
1. Since current location has not yet been initialised, Confab will display an error. Ignore this and click "Go to login" in the top right of the widget.
1. Enter the email that you entered under `UserRoles:Admin` previously
1. Since SMTP probably hasn't been configured yet, you can instead find the login code printed out to the Confab console. Use this to login.
1. Once successfully logged in, open the Admin Panel and initialise commenting at the current location.

## Building Confab

### Prerequisites

- [.NET 6.0 SDK](https://learn.microsoft.com/en-us/dotnet/core/install/){:target="_blank"} (1)
    { .annotate }

    1.  Install on Ubuntu with the following command: 
    ```bash
    sudo apt-get update && sudo apt-get install -y dotnet-sdk-6.0
    ```

- [NodeJS](https://nodejs.org/en/download){:target="_blank"} (1)
    { .annotate }

    1.  For Ubuntu, [Fast Node Manager (fnm)](https://github.com/Schniz/fnm#using-a-script-macoslinux) usually works well, install via the following commands:
    ```bash
    curl -fsSL https://fnm.vercel.app/install | bash
    source /home/ubuntu/.bashrc
    fnm install 20.10.0
    ```

- [Python](https://www.python.org/downloads/){:target="_blank"} (1)
    { .annotate }

    1.  Python is necessary to run build scripts, but isn't strictly necessary for a fully manual build

Download the project files by cloning the Github repository: `git clone https://github.com/{{variables.CONFAB_GITHUB_LOCATION}}.git --recursive`

### Option 1 - Build Script

Confab comes with a build script that automates the build process. 
```python
python3 ./Confab/confab-builder.py # (1)!
```
{ .annotate }

1.  CLI Options:
```
usage: confab_builder [-h] [-m {ui,backend,full}] [-c] [-p PLATFORM] [-b]

options:
  -h, --help            show this help message and exit
  -m {ui,backend,full}, --mode {ui,backend,full}
                        Choose whether to build UI, backend, or both
  -c, --clean           Clean build
  -p PLATFORM, --platform PLATFORM
                        Compile .NET backend for specific platform (leave empty for current platform).
  -b, --bundle-runtime  Bundle .NET runtime (will run without requiring .NET runtime to be installed, but will increase build size)
```
        
### Option 2 - Build Manually

#### Step 1: Build Confab UI

=== "Using Script"

    1. Change bash shell directory to the Confab UI folder (`Confab/ConfabUI`)
    2. Run UI build script
    ```bash
    python3 build.py
    ```

=== "Manually"

    1. Change bash shell directory to the Confab UI folder (`Confab/ConfabUI`)
    2. Install dependencies
    ```bash
    npm install
    ```
    3. Build UI
    ```bash
    npm run build
    ```
    4. Move build files
    ```bash
    mv ./dist/* ../Confab/wwwroot
    ```

#### Step 2: Compile Email Templates

=== "Using Script"

    1. Change bash shell directory to the Email template folder (`Confab/email_designs`)
    2. Run UI build script
    ```bash
    python3 compile.py
    ```

=== "Manually"

    1. Change bash shell directory to the Email template folder (`Confab/email_designs`)
    2. Install dependencies
        ```bash
        npm install
        ```
    
    3. Compile email templates
    ```bash
    ./node_modules/.bin/mjml ./templates/*.mjml -o ../Confab/App_Data/email_templates/  --config.minify true --config.beautify false 
    ```

#### Step 3: Compile Confab Backend

1. Change bash shell directory to the backend folder (`Confab/Confab`)
2. Build Confab
```bash
dotnet publish --configuration Release --output ../App
```
3. Move scripts to build directory
```bash
mv ../scripts/* ../App
```

---

🎉 That's it! 🎉

Your build of Confab should now be found at `Confab/App`.
