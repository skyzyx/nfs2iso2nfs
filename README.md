# nfs2iso2nfs

`nfs2iso2nfs` is a CLI tool which converts `*.nfs` files to an `*.iso` and back.

> [!TIP]
> This is a version of `nfs2iso2nfs` that is cross-platform instead of Windows-only. Works on macOS, Linux, and Windows.

## Authorship

1. <https://github.com/sabykos/nfs2iso2nfs>
1. <https://github.com/piratesephiroth/nfs2iso2nfs>
1. <https://github.com/FIX94/nfs2iso2nfs>
1. <https://github.com/rhinoswirl/nfs2iso2nfs>
1. <https://github.com/skyzyx/nfs2iso2nfs>

## Compiling from source

Choose **ONE** of the below choices. You should be _generally_ familiar with how to compile software.

### Using Mono

1. Install [Mono](https://www.mono-project.com/download/stable/).

1. Here are some [basic examples](https://www.mono-project.com/docs/getting-started/mono-basics/) to ensure that Mono was installed correctly.

1. Compile the `.cs` file into an executable.

    ```bash
    csc nfs2iso2nfs.cs
    ```

    This will result in a file called `nfs2iso2nfs.exe`.

1. Even though `.exe` is a Windows-centric file extension, you can still run this binary on macOS and Linux platforms by prepending the command with `mono`.

    ```bash
    mono ./nfs2iso2nfs.exe -help
    ```

### Using Modern .NET

1. Download the [.NET SDK](https://dotnet.microsoft.com/en-us/download) for your platform and CPU.

1. Compile the `.cs` file into an executable (using the project definition in `nfs2iso2nfs.csproj`).

    ```bash
    dotnet publish
    ```

    This will result in a file called `nfs2iso2nfs` a few subdirectories deep under the `bin/` directory. The one we're looking for is inside the `publish/` subdirectory. On Linux/macOS, move the binary from the subdirectory to the current directory with the following command:

    ```bash
    find ./bin/ -type f -name nfs2iso2nfs | grep -i "publish" | xargs -I% mv -v "%" .
    ```

1. Run this binary on macOS and Linux platforms as usual.

    ```bash
    ./nfs2iso2nfs -help
    ```

## Usage

### Set up

> [!CAUTION]
> I've made changes to the default directory structure compared to previous forks of this software. Please pay attention to the HELP text.

When downloading Wii games from the Wii U eShop, and then you extract that game back to your computer, you will have a set of directories which look like this:

```plain
Super Awesome Game!/
├── code/
│   ├── fw.img
│   ├── htk.bin
│   └── ...
├── content/
│   ├── assets/
│   │   └── shaders/
│   │       └── cafe/
│   │           └── ...
│   ├── hif_000000.nfs
│   ├── hif_000001.nfs
│   ├── hif_000002.nfs
│   └── ...
└── meta/
    ├── bootDrcTex.tga
    ├── bootMovie.h264
    ├── iconTex.tga
    ├── meta.xml
    └── ...
```

In this version of `nfs2iso2nfs`, you would copy/move `nfs2iso2nfs` inside the `Super Awesome Game!/` directory, instead of the `Super Awesome Game!/content/` directory. (This was not explained well in the original help text. This has been fixed.)

You will also need to copy `wii_common_key.bin` into the same directory as `nfs2iso2nfs` (or else specify a different path with the `-wiikey` option).

### Running

> [!NOTE]
> For the examples below, I'm going to use `./nfs2iso2nfs` because it's shorter. If you followed the instructions for Mono, just replace `./nfs2iso2nfs` in the commands with `mono ./nfs2iso2nfs.exe`. Otherwise, they are exactly the same.

1. Make sure you're inside the game directory.

    ```bash
    cd "Super Awesome Game!/"
    ```

1. Run the command in _decrypt_ mode in order to produce an `.iso`. This will take several minutes depending on the size of the game and the speed of your hard drive.

    ```bash
    ./nfs2iso2nfs -dec
    ```

    <details>
    <summary>See sample output…</summary><br>

    ```plain
    +++++ NFS2ISO +++++

    Searching for AES key file...
    AES key file found!
    Wii common key not found in source code. Looking for file...
    Wii Common Key file found!

    Looking for .nfs files...
    26 .nfs files found!
    Joining .nfs files...

    Processing hif_000000.nfs...
    Processing hif_000001.nfs...
    Processing hif_000002.nfs...
    […snip…]

    Decrypting hif.nfs...

    256 MB processed...
    512 MB processed...
    768 MB processed...
    […snip…]

    Unpacking nfs...

    3 parts found...
    Writing zero segment 0 of size 0x0
    Writing data segment 0 of size 0x8000
    Writing zero segment 1 of size 0x38000
    […snip…]

    Read partition table...

    Number of 1. partitions: 1
    Partition info table offset: 0x40020
    Number of 2. partitions: 0
    […snip…]

    Data partition at offset: 0xf800000

    Partition size: 0x193ad0000
    Write game partition 0...
    256 MB processed...
    512 MB processed...
    768 MB processed...
    […snip…]

    Writing zeros...
    256 MB processed...
    512 MB processed...
    768 MB processed...
    […snip…]
    ```

    </details>

## Playing the game

Since this is a Wii ISO now, you should be able to play it using an emulator such as [Dolphin](https://dolphin-emu.org).
