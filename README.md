# NES TBAS Interpreter V1

This NES ROM is a TBAS (Tis But a Scratch) interpreter and is also a ZIP file that contains its own source code.

**_ NES TBAS Interpreter V1 is created by Vi Grey (https://vigrey.com) <vi@vigrey.com> and is licensed under the BSD 2-Clause License._**

#### License:
Copyright (C) 2018, Vi Grey
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions
are met:

1. Redistributions of source code must retain the above copyright notice, this list of conditions and the following disclaimer.
2. Redistributions in binary form must reproduce the above copyright notice, this list of conditions and the following disclaimer in the documentation and/or other materials provided with the distribution.

THIS SOFTWARE IS PROVIDED BY AUTHOR AND CONTRIBUTORS \`\`AS IS'' AND
ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
ARE DISCLAIMED. IN NO EVENT SHALL AUTHOR OR CONTRIBUTORS BE LIABLE
FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL
DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS
OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION)
HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT
LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY
OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF
SUCH DAMAGE.



#### Description:

This neszip example will create an NES ROM file that is also a ZIP file that contains its own source code.  Using the source code to build a the NES ROM file will also embed the ZIP file within it as well, making another NES ROM file that is also a ZIP file that contains its own source code.  This is a recursive NES ROM and ZIP polyglot file.

The NES ROM can be burned onto a cartridge and it will still work as an NES game while retaining the ZIP file, so if the NES ROM is ripped from the cartridge, that resulting NES ROM file would also be a ZIP file.

#### Platforms:
- GNU/Linux

#### Build Dependencies:
- asm6 _(You'll probably have to build asm6 from source.  Make sure the asm6 binary is named **asm** and that the binary is executable and accessible in your PATH. The source code can be found at http://3dscapture.com/NES/asm6.zip)_
- zip
- Python 3

#### Build NES ROM:

From a terminal, go to the the main directory of this project (the directory this README.md file exists in), you can then build the NES ROM with the following command.

    $ make

The resulting NES ROM will be located at **bin/nes-tbas-v1.nes**

#### Cleaning Build Environment:

If you used `make` to build the NES ROM, you can run the following command to clean up the build environment.

    $ make clean

#### Unzipping the ROM:

Your zip file extractor should be able to extract this file, although you may have to change the extension from **.nes** to **.zip** to make your zip file extractor work.

#### Burning the ROM onto an NES Cartridge:

This NES ROM works on an NES-NROM-256 board.

#### Interpreter Instructions:

- **[** - If mCell[mPtr] == 0, then ePtr++ until eCell[ePtr - 1] == **]** or the until ePtr has reached the end of eCell, else ePtr++.  **[** and **]** operators may be nested.
- **]** - ePtr-- until eCell[ePtr] == **[**.  Will cause an infinite loop if there is no matching **[**.  **[** and **]** operators may be nested.
- **<** - If mPtr > 0, then mPtr--.
- **>** - If mPtr < 255, then mPtr++.
- **+** - If mCell[mPtr] < 255, then mCell[mPtr]++.
- **\-** - If mCell[mPtr] > 0, then mCell[mPtr]++.
- **=** - IO Mode = mCell[mPtr] if mCell[mPtr] is a valid IO Mode value.
- **?** - Carries out the corresponding IO Mode.

#### IO Modes:

- **0** - Writes mCell[mPtr] value as a decimal value.
- **1** - Writes mCell[mPtr] value as an ASCII value.
- **2** - Reads input byte provided by user.
- **3** - Appends mCell[mPtr] to FIFO Buffer.
- **4** - Clears FIFO Buffer.
- **5** - Executes FIFO Buffer as 6502 Machine Code.

#### Buffer Limitations:

NES TBAS Interpreter V1 uses multiple buffers.  Each buffer is only 256 bytes long and after a buffer is full, corresponding operations that add to that buffer will be ignored.

- **eCell Buffer** - This is the TBAS string that will be executed.  It starts at NES Memory Address 0x300.
- **Print Buffer** - This is the buffer for data that is to be printed in the interpreter prompt screen.  It starts at NES Memory Address 0x400.
- **mCell Buffer** - This is the working memory of an executing TBAS string.  It starts at NES Memory Address 0x500.
- **FIFO Buffer** - This is a First In First Out buffer, that when executed, runs user submitted 6502 Machine Code.  It starts at NES Memory Address 0x500.

#### CAUTION:

Due to the fact that the user can set 6502 Machine Code in the FIFO Buffer and execute it, it is important to note that setting bit 6 [-X------] of NES Memory Address 0x2000 to 1 can physically damage your NES.  Use caution when executing code from the FIFO Buffer.
