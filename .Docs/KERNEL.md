*** Auto generated by docgen.cmd ***  

## Copyright 
Copyright 2015 - 2020, Remy Gibert and the A2osX contributors. 

# Shift  

## ASM  
A = argument index.  

## RETURN VALUE   
CC : success  
Y,A = PTR To Arg[A]  
CS : Out Of Bound  

# ArgV  

## ASM  
A = argument index.  

## RETURN VALUE   
CC : success  
Y,A = PTR To Arg[A]  
CS : Out Of Bound  

# Arg2ArgV  
Expand String and convert to StrV List  

## C  
short int arg2argv(char* args, char* argv[])  

## ASM  
`>PUSHW args`  
`>PUSHW argv`  
`>SYSCALL Arg2ArgV`  

## RETURN VALUE  
A = Arg count  

# ArgVDup  

## ASM  
 Y,A = Src StrV  

## RETURN VALUE  
 X = hMem of new StrV  
 A = Str Count  

# LoadDrv  

## ASM  
**In:**  
 Y,A = PTR to "NAME.DRV [PARAM]" C-String  

## RETURN VALUE  
none  

# InsDrv  

## C  
`void * insdrv (void * src, void * crvcsstart, void * drvcssize, void * drvend);`  

## ASM  
**In:**  
`>PUSHW DRV.END`  
`>PUSHW DRV.CS.SIZE`  
`>PUSHW DRV.CS.START`  
`>LDYA L.SRC`  
`>SYSCALL insdrv`  

## RETURN VALUE  
Y,A = Ptr to installed driver  

# GetDevByName  
 Y,A = Ptr to device name (C-String)  

## RETURN VALUE  
CC = OK, CS = ERROR  
Y,A = FD  
X = hFD  

# MkFD  

## C  
`short int mkfd(short int type, );`  

## ASM  
`>PUSHB DevID`  
`>PUSHW S.DIB`  
`>SYSCALL MkFD`  

## RETURN VALUE  

# MKDev  
Create a hDEV  

## C  
`hDEV mkdev (S.FD *fd, const char *devname)`  

## ASM  
`>PUSHW fd`  
`>PUSHW devname`  
`>SYSCALL mkdev`  

## RETURN VALUE  
 A = hDEV  

# IOCTL  

## C  
`int ioctl(short int hFD, short int request, void *param);`  

## ASM  
`>PUSHB hDEV`  
`>PUSHB request`  
`>PUSHW param`  
`>SYSCALL IOCTL`  

## RETURN VALUE  
 Y,A = ...  

# OpenDir  

## C  
`short int hDIR opendir (const char * dirpath);`  

## ASM  
`>LDYA dirpath`  
`>SYSCALL opendir`  

## RETURN VALUE  
 CC : success  
  A = hDIR  
 CS : error  
  A = EC  

# ReadDir  

## C  
`int readdir (int hDIR, S.DIRENT * dirent);`  

## ASM  
`>PUSHW dirent`  
`lda hDIR`  
`>SYSCALL readdir`  

## RETURN VALUE  
 CC : success  
  X = hDIRENT  
  Y,A = PTR to S.DIRENT  
 CS : error  
  A = EC  
  note : A = 0 means no more entry  

# CloseDir  

## C  
`void closedir(hDIR);`  

## ASM  
`lda hDIR`  
`>SYSCALL closedir`  

## RETURN VALUE  
 none, always succeed.  

# SetEnv  
Change or add an environment variable  

## C / CSH  
`int setenv(const char *name, const char *value);`  

## ASM  
`>PUSHW name`  
`>PUSHW value`  
`>SYSCALL setenv`  

## RETURN VALUE  

# GetEnv  
searches the environment list to find the environment variable name,   
and returns a pointer to the corresponding value string.  

## C / CSH  
`char *getenv(const char *name, char *value);`  

## ASM  
`>PUSHW name`  
`>PUSHW value`  
`>SYSCALL getenv`  

## RETURN VALUE  
 CC : Y,A = PTR to VALUE (C-String)  
 CS : not found  

# PutEnv  
Change or add an environment variable, string is 'NAME=VALUE'  

## C / CSH  
`int putenv(char *string);`  

## ASM  
**In:**  
`>PUSHW string`  
`>SYSCALL putenv`  

## RETURN VALUE  

# UnsetEnv  
Remove an environment variable  

## C / CSH  
`int unsetenv(const char *name);`  

## ASM  
`>PUSHW name`  
`>SYSCALL unsetenv`  

## RETURN VALUE  

# Add32,Sub32,Mul32,IMul32,Div32,IDiv32,Mod32,IMod32,Cmp32,ICmp32  
Return X+Y, X-Y, X*Y, X/Y, X mod Y....  

## ASM  
**In:**  
`>PUSHL X (long)`  
`>PUSHL Y (long)`  
`>FPU add32`  
`...`  

## RETURN VALUE  
 On stack (long)  

# FAdd,FSub,FMul,FDiv,FPwr  
Return X*Y, X/Y, X+Y, X-Y  

## C  
`float pwr ( float x, float y);`  

## ASM  
**In:**  
`>PUSHF X (float)`  
`>PUSHF Y (float)`  
`>FPU fmul`  
`>FPU fdiv`  
`>FPU fmod`							TODO  
`>FPU fadd`  
`>FPU fsub`  
`>FPU fpwr`  

## RETURN VALUE  
 On stack (float)  

# Log,Sqr,Exp,Cos,Sin,Tan,ATan  
Return Log(x), Sqr(x), E^X, Cos(x), Sin(X), Tan(x), ATan(x)  

## C  
`float log ( float x);`  
`float sqr ( float x);`  
`float exp ( float x);`  
`float cos ( float x);`  
`float sin ( float x);`  
`float tan ( float x);`  
`float atan ( float x);`  

## ASM  
**In:**  
`>PUSHF x (Float)`  
`>FPU log`  

## RETURN VALUE  
 On stack (Float)  

# float  
Return 'floated' long  

## C  
`float f = (float)l;  

## ASM  
**In:**  
`>PUSHL l` (long)  
`>FPU float`  

## RETURN VALUE  
 On stack (float)  

# lrintf  
Return float rounded into a long  

## C  
`long int lrintf (float x);`  

## ASM  
**In:**  
`>PUSHF x`  
`>FPU lrintf`  

## RETURN VALUE  
 On stack (long)  

## MD5  
Return MD5 Hash for input String  

# C  
`void md5 (const char* str, char* digest);`  

# ASM  
`>PUSHW str`  
`>PUSHW digest`  
`>SYSCALL md5`  

## RETURN VALUE  
CC  

## MD5Init  
Initialize a MD5 computation  

# C  
`short int md5init();`  

# ASM  
`>SYSCALL MD5Init`  

## RETURN VALUE  
A = hMem To S.MD5  

## MD5Update  
Add Data to MD5 computation  

# C  
`void md5update (short int md5, char* data, int len);`  

# ASM  
`>PUSHB md5`  
`>PUSHW data`  
`>PUSHW len`  
`>SYSCALL MD5Update`  

## RETURN VALUE  

## MD5Finalize  

# C  
`void md5finalize (short int md5, char* digest);`  

# ASM  
`>PUSHB md5`  
`>PUSHW digest`  
`>SYSCALL MD5Finalize`  

## RETURN VALUE  

# Realloc  

## C  
`void *realloc(short int hMem, int size);`  

## ASM  
`>PUSHB hMem`  
`>PUSHW size`  
`>SYSCALL realloc`  

## RETURN VALUE  
 YA = ptr  
 X = hMem  

# GetMem  
 Y,A = Size Requested  

## RETURN VALUE  
 CC : success  
  YA = PTR to Mem (Uninitialised)  
*	X = hMem  
 CS :  
  A = EC  

# FreeMem  
 A = hMem To Free  

## RETURN VALUE  
 none.  
 (X unmodified)  

# GetMemPtr  
A = hMem  

## RETURN VALUE  
Y,A = PTR to MemBlock  
(X unmodified)  

# NewStkObj  
 Y,A = Size Requested  

## RETURN VALUE  
 CC : success  
  YA = PTR to Mem (Uninitialised)  
*	X = hMem  
 CS :  
  A = EC  

# GetStkObjPtr  

## ASM  
`lda hStkObj`  
`>SYSCALL GetStkObjPtr`  

## RETURN VALUE  

# FreeStkObj  
 A = hMem To Free (AUX Memory)  

## RETURN VALUE  
 none.  
 (X,Y unmodified)  

# GetStkObj  

## C  
`int *ptr getstkobj (short int hStkObj);`  

## ASM  
`lda hStkObj`  
`>SYSCALL GetStkObj`  

## RETURN VALUE  
 CC : success  
  X = hMem  
  Y,A = ptr  
 CS : error  
  A = EC  

# Online  
Get ProDOS Volume Info  

## C  
`int online(short int volid, void *buf);`  

## ASM  
`>PUSHB volid`  
`>PUSHW buf`  
`>SYSCALL Online`  

## RETURN VALUE  

# ChTyp  
Change Type of a ProDOS File  

## C  
`int chtyp(const char *filepath, short int filetype);`  

## ASM  
`>PUSHW filepath`  
`>PUSHB filetype`  
`>SYSCALL ChTyp`  

## RETURN VALUE  

# ChAux  
Change AuxType of a ProDOS File  

## C  
`int chaux(const char *filepath, int auxtype);`  

## ASM  
`>PUSHW filepath`  
`>PUSHW auxtype`  
`>SYSCALL ChAux`  

## RETURN VALUE  

# SetAttr  
Change Attributes of a ProDOS File  

## C  
`int setattr(const char *filepath, short int attributes);`  

## ASM  
`>PUSHW filepath`  
`>PUSHB attributes`  
`>SYSCALL setattr`  

## RETURN VALUE  

# ExecL  

## C / CSH  
`short int execl(const char *cmdline, short int flags);`  

## ASM  
`>PUSHW cmdline`  
`>PUSHB flags`  
`>SYSCALL execl`  

## RETURN VALUE  
A = Child PSID  

# ExecV  

## C / CSH  
`short int execv(const char* argv[], short int flags);`  

## ASM  
`>PUSHW argv`  
`>PUSHB flags`  
`>SYSCALL execv`  

## RETURN VALUE  
A = Child PSID  

# Fork  

## C  
`short int fork();`  

## ASM  
`>SYSCALL fork`  

## RETURN VALUE  
A = Child PSID  

# Kill  

## C  
`int kill(short int pid, short int sig);`  

## ASM  
`>PUSHB pid`  
`>PUSHB sig`  
`>SYSCALL kill`  

## RETURN VALUE  

# LoadStkObj  
Load a file in AUX memory (Stock Objects)  
 PUSHW = PATH (Handled by....  
 PUSHB = MODE  ...  
 PUSHB = TYPE  ...  
 PUSHW = AUXTYPE ...FOpen)  

## RETURN VALUE  
 Y,A = File Length  
 X = hMem of Loaded Object in AUX mem  

# LoadTxtFile  
Load TXT a file in memory (with ending 0)  

## C  
`int loadtxtfile ( const char * filename );`  

## ASM  
**In:**  
`>LDYA filename`  
`>SYSCALL loadtxtfile`  

## RETURN VALUE  
 Y,A = File Length (without ending 0)  
 X = hMem of Loaded File  

# LoadFile  
Load a file in memory  

## C  
`int loadfile ( const char * filename, short int flags, short int ftype, int auxtype );`  

## ASM  
**In:**  
`>PUSHW filename`  
`>PUSHB flags`  
`>PUSHB ftype`  
`>PUSHW auxtype`  
`>SYSCALL loadfile`  

## RETURN VALUE  
 Y,A = File Length  
 X = hMem of Loaded File  

# FileSearch  
Search a file in the provided PATH list  
And return, if found, the full path to it.  

## C  
`int filesearch( char *filename, char *searchpath, char *fullpath, stat *filestat);`  

## ASM  
**In:**  
`>PUSHW filename`  
`>PUSHW fullpath`  
`>PUSHW searchpath`  
`>PUSHW filestat`  
`>SYSCALL filesearch`  

## RETURN VALUE  
CC : success  
DstBuf = FilePath  
DstStat = S.STAT  
CS : not found  

# GetMemStat  
**In:**  
 Y,A = Ptr to 24 bytes buffer  

## RETURN VALUE  
 Buffer filled with memory stats  

# GetPWUID  

## C  
`int getpwuid(short int uid, S.PW *passwd);`  

## ASM  
`PUSHB uid`  
`>PUSHW passwd`  
`>SYSCALL getpwuid`  

## RETURN VALUE  

# GetGRGID  

## C  
`int getgrgid(short int gid, S.GRP *group);`  

## ASM  
`>PUSHB gid`  
`>PUSHW group`  
`>SYSCALL getpwname`  

## RETURN VALUE  

# CloseSession  

## C  
`int closesession(short int hSID);`  

## ASM  
`>PUSHB hSID`  
`>SYSCALL CloseSession`  

## RETURN VALUE  

# OpenSession  

## C  
`short int hSID opensession(const char *name, const char *passwd);`  

## ASM  
`>PUSHW name`  
`>PUSHW passwd`  
`>SYSCALL OpenSession`  

## RETURN VALUE  

# GetPWName  

## C  
`int getpwname(const char* name, S.PW *passwd);`  

## ASM  
`>PUSHW name`  
`>PUSHW passwd`  
`>SYSCALL getpwname`  

## RETURN VALUE  

# GetGRName  

## C  
`int getgrgid(const char* name, S.GRP *group);`  

## ASM  
`>PUSHW name`  
`>PUSHW group`  
`>SYSCALL getpwname`  

## RETURN VALUE  

# PutPW  

## C  
`int putpw(S.PW* passwd);`  

## ASM  
`>PUSHW passwd`  
`>SYSCALL putpw`  

## RETURN VALUE  

# PutGR  

## C  
`int putgr(S.GRP *group);`  

## ASM  
`>PUSHW group`  
`>SYSCALL putgr`  

## RETURN VALUE  

# SListGetData  

## ASM  
`>PUSHB hSList`  
`>PUSHW KeyID`  
`>PUSHW DataPtr` (0 if KERNEL should allocate a buffer)  
`>PUSHW DataLen` (Data bytes to return, 0 if String mode)  
`>PUSHW DataOfs` (Start offset in Data)  
`>SYSCALL SListGetData`  

## RETURN VALUE  
 Y,A = Byte Count  
 X = hMem (if DataPtr = 0)  

# SListAddData  

## ASM  
`>PUSHB hSList`  
`>PUSHW KeyID`  
`>PUSHW DataPtr`  
`>PUSHW DataLen`  
`>SYSCALL SListAddData`  

## RETURN VALUE  

# SListSetData  

## ASM  
`>PUSHB hSList`  
`>PUSHW KeyID`  
`>PUSHW DataPtr`  
`>PUSHW DataLen`  
`>SYSCALL SListSetData`  

## RETURN VALUE  

# SListGetByID  

## ASM  
`>PUSHB hSList`  
`>PUSHW KeyID`  
`>PUSHW KeyPtr`  
`>SYSCALL SListGetByID`  

## RETURN VALUE  
 Y,A = Next KeyID  

# SListNewKey  

## ASM  
`>PUSHB hSList`  
`>PUSHW KeyPtr`  
`>SYSCALL SListNewKey`  

## RETURN VALUE  
 Y,A = KeyID  
 X = KeyLen  

# SListLookup  

## ASM  
`>PUSHB hSList`  
`>PUSHW KeyPtr`  
`>SYSCALL SListLookup`  

## RETURN VALUE  
 Y,A = KeyID  
 X = Key Length  

# SListFree  

## ASM  
`lda hSList`  
`>SYSCALL SListFree`  

## RETURN VALUE  

# SListNew  

## ASM  
`>SYSCALL SListNew`  

## RETURN VALUE  
A=hSList  

# ChMod  
change permissions of a file  

## C  
`int chmod(const char *pathname, int mode);`  

## ASM  
`>PUSHW pathname`  
`>PUSHW mode`  
`>SYSCALL chmod`  

## RETURN VALUE  

# FStat  
Return information about a hFILE  

## C  
`int fstat(short int hFILE, struct stat *statbuf);`  

## ASM  
`>PUSHB hFILE`  
`>PUSHW statbuf`  
`>SYSCALL fstat`  

## RETURN VALUE  

# Stat  
Return information about a file  

## C  
`int stat(const char *pathname, struct stat *statbuf);`  

## ASM  
`>PUSHW pathname`  
`>PUSHW statbuf`  
`>SYSCALL stat`  

## RETURN VALUE  

# MKDir  
create a directory  

## C  
`int mkdir(const char *pathname, int mode);`  

## ASM  
`>PUSHW pathname`  
`>PUSHW mode`  
`>SYSCALL mkdir`  

## RETURN VALUE  
CC : success  
CS : error  
A = EC  

# MKFIFO  
return a pathname to a new FIFO  

## C  
`hFILE mkfifo(const char *pathname, int mode);`  

## ASM  
`>PUSHW pathname`  
`>PUSHW mode`  
`>SYSCALL mkfifo`  

## RETURN VALUE  
CC = OK, CS = ERROR  
A = hFILE  

# MkNod  
Create a special or ordinary file.  
(CDEV, BDEV, DSOCKS, SSOCK, PIPE)  

## C  
`hFILE mknod(const char *pathname, int mode, hFD fd);`  

## ASM  
`>PUSHW pathname`  
`>PUSHW mode`  
`>PUSHB fd`  
`>SYSCALL mknod`  

## RETURN VALUE  
CC = OK, CS = ERROR  
A = hFILE  

# pipe  

## C  
`hFD pipe(int size);`  

## ASM  
`>LDYA size`  
`>SYSCALL pipe`  

## RETURN VALUE  
CC = OK, CS = ERROR  
A = hFD  

# fputc (BLOCKING)  
Print A (char) to hFILE  

## C  
`int fputc ( hFILE stream , short int character );`  

## ASM  
**In:**  
`>PUSHB stream`  
`>PUSHB character`  
`>SYSCALL fputc`  

## RETURN VALUE  
CC = success  

# putchar (BLOCKING)  
Print A (char) to StdOut  

## C  
`int putchar ( short int character );`  

## ASM  
**In:**  
`lda character`  
`>SYSCALL putchar`  

## RETURN VALUE  
CC = success  

# puts (BLOCKING)  
Write Str to StdOut, appends '\r\n'  

## C  
`int puts ( const char * str );`  
**In:**  

## ASM  
`>LDYAI str`  
`>SYSCALL PutS`  

## RETURN VALUE  
CC = success  

# fputs (BLOCKING)  
Write Str to hFILE  

## C  
`int fputs (hFILE stream, const char * str );`  

## ASM  
**In:**  
`>PUSHB stream`  
`>PUSHW str`  
`>SYSCALL fputs`  

## RETURN VALUE  
CC = success  

# fgets (BLOCKING)  
read bytes from stream into the array  
pointed to by s, until n-1 bytes are read, or a <newline> is read and  
transferred to s, or an end-of-file condition is encountered. The  
string is then terminated with a null byte.  

## C  
`char *fgets(hFILE stream, char * s, int n);`  

## ASM  
**In:**  
`>PUSHB hFILE`  
`>PUSHW s`  
`>PUSHW n`  
`>SYSCALL fgets`  

## RETURN VALUE  
 Y,A: s  
CC = success  

# getchar (BLOCKING)  
Get char from StdIn  

## C  
`short int getchar ( );`  

## ASM  
**In:**  
`>SYSCALL getchar`  

## RETURN VALUE  
 CC = success  
  A = char  

# getc (BLOCKING)  
Get char from Node  

## C  
`short int getc ( short int stream );`  

## ASM  
**In:**  
`lda stream`  
`>SYSCALL getc`  

## RETURN VALUE  
 CC = success  
  A = char  

# ungetc  
push byte back into input stream  

## C  
`short int ungetc(short int c, short int stream);`  

## ASM  
`>PUSHB c`  
`>PUSHB stream`  
`>SYSCALL ungetc`  

## RETURN VALUE  
 CC = success  
  A = char  

# FOpen  
Open a file  

## C  
`short int fopen ( const char *filename, short int flags, short int ftype, int auxtype );`  
**In:**  

## ASM  
`>PUSHW filename`  
`>PUSHB flags`  
 + O.RDONLY : if R and exists -> ERROR  
 + O.WRONLY : if W and exists -> CREATE  
 + O.TRUNC : Reset Size To 0  
 + O.APPEND : Append  
 + O.TEXT   : Open/Append in Text mode  
 + O.CREATE : Create if not exists  
`>PUSHB ftype`  
`>PUSHW auxtype`  
TODO: replace flags/ftype/auxtype with mode="w+,t=TYP,x=AUXTYPE"  
 + r  = O_RDONLY  
 + r+ = O_RDWR  
 + w  = O_WRONLY | O_CREAT | O_TRUNC  
 + w+ = O_RDWR | O_CREAT | O_TRUNC  
 + a  = O_WRONLY | O_CREAT | O_APPEND  
 + a+ = O_RDWR | O_CREAT | O_APPEND  
 + ,t=123 or t=$ff or t=TXT  
 + ,x=12345 or x=$ffff  

## RETURN VALUE  
 CC : A = hFILE  
 CS : A = EC  

# FClose  
Close a file  

## C  
`int fclose ( short int stream );`  

## ASM  
**In:**  
`lda stream`  
`>SYSCALL FClose`  

## RETURN VALUE  

# FRead (BLOCKING)  
Read bytes from file  

## C  
`int fread (short int stream, void * ptr, int count );`  

## ASM  
**In:**  
`>PUSHB stream`  
`>PUSHW ptr`  
`>PUSHW count`  
`>SYSCALL fread`  

## RETURN VALUE  
 Y,A = Bytes Read  

# FWrite (BLOCKING)  
Write bytes to file  

## C  
`int fwrite ( short int stream, const void * ptr, int count );`  

## ASM  
**In:**  
`>PUSHB stream`  
`>PUSHW ptr`  
`>PUSHW count`  
`>SYSCALL fwrite`  

## RETURN VALUE  
 Y,A = Bytes Written  

# FFlush  

## C  
`int fflush( short int stream );`  

## ASM  
**In:**  
`lda stream`  
`>SYSCALL fflush`  

# FSeek  
Set the file-position indicator for hFILE  

## C  
`int fseek( short int stream, long offset, short int whence );`  

## ASM  
**In:**  
`>PUSHB stream`  
`>PUSHL offset`  
`>PUSHB whence`  
`>SYSCALL fseek`  

# FEOF  
Test the end-of-file indicator for hFILE  

## C  
`int feof( short int stream );`  

## ASM  
**In:**  
`lda stream`  
`>SYSCALL feof`  

## RETURN VALUE  
 CC :  
 A = $ff EOF  
 A = 0 NOT EOF  
 CS :  

# FTell  
Return the current value of the file-position indicator  

## C  
`long ftell( short int stream );`  

## ASM  
**In:**  
`lda stream`  
`>SYSCALL ftell`  

## RETURN VALUE  
On stack (long)  

# Remove  
Remove a file or directory  

## C  
`int remove ( const char *pathname );`  

## ASM  
**In:**  
`>LDYA pathname`  
`>SYSCALL remove`  

## RETURN VALUE  

# Rename  
Rename a file  

## C  
`int rename ( const char * oldpath, const char * newpath );`  

## ASM  
**In:**  
`>PUSHW oldpath`  
`>PUSHW newpath`  
`>SYSCALL rename`  

## RETURN VALUE  

# PrintF (BLOCKING)  

# FPrintF (BLOCKING)  

# SPrintF  
Prints C-Style String  

## C  
`int printf ( const char *format, ... );`  
`int fprintf ( short int stream, const char *format, ... );`  
`int sprintf ( char *str, const char *format, ... );`  

## ASM  
**In:**  
PrintF : (example is for printing Y,A as integer : format="%I", 2 bytes)  
`>PUSHW format`  
`>PUSHW i`  
`...`  
`>PUSHBI 2`	#bytecount  
`>SYSCALL PrintF`  
FPrintF :  
`>PUSHB hFILE`  
`>PUSHW format`  
`>PUSHW i`  
`...`  
`>PUSHBI 2`	#bytecount  
`>SYSCALL fprintf`  
SPrintF :  
`>PUSHW str`  
`>PUSHW format`  
`>PUSHW i`  
`...`  
`>PUSHBI 2`	#bytecount  
`>SYSCALL sprintf`  

## RETURN VALUE  
CC : success, Y,A = bytes sent  
CS : error, A = code from Output  
Specifiers :  
+ %b : pull 1 byte to Print BIN  
+ %d : pull 1 byte unsigned DEC 0..255  
+ %D : pull 2 bytes unsigned DEC 0..65535  
+ %u : pull 4 bytes long unsigned DEC 0..4294967295  
+ %e : pull 5 Bytes float (-)1.23456789e+12  
+ %f : pull 5 Bytes float (-)3.1415  
+ %h : pull 1 byte to Print HEX  
+ %H : pull 2 bytes to Print HEX  
+ %i : pull 1 byte to Print signed DEC -128..127  
+ %I : pull 2 bytes to Print signed DEC -32768..32767  
+ %L : pull 4 bytes signed DEC -2147483648..2147483647  
+ %s : pull 2 bytes ptr to C-Style String  
+ %S : pull 2 bytes ptr to P-Style String  
+ \b : Print 'BS' (08)  
+ \e : Print 'ESC' ($1B,27)  
+ \f : Print 'FF' ($0C,12)  
+ \n : Print 'LF' ($0A,10)  
+ \r : Print 'CR' ($0D,13)  
+ \t : Print 'TAB' ($09,09)  
+ \v : Print 'VT' ($0B,11)  
+ \xHH : Print byte with hexadecimal value HH (1 to 2 digits)  
+ \\\\ : Print \  
+ \\% : Print %  

Modifiers for len and padding :  
+ %d	  : '9'  '12'  
+ %2d	  : ' 9' '12'  
+ %02d  : '09' '12'  
+ %11s  : 'ABCDEFGH   '  
+ %011s : 'ABCDEFGH000'  
+ %2f	  :	'3.14'  


# ScanF (BLOCKING)  

# FScanF (BLOCKING)  

# SScanF  
Read formatted data from string  

## C  
`int scanf( const char *format, ... );`  
`int fscanf( short int stream, const char *format, ... );`  
`int sscanf ( const char *s, const char *format, ... );`  

## ASM  
**In:**  
ScanF :  
`>PUSHW format`  
`>PUSHW ptr`  
`...`  
`>PUSHB bytecount`  
`>SYSCALL scanf`  
FScanF :  
`>PUSHB stream`  
`>PUSHW format`  
`>PUSHW ptr`  
`...`  
`>PUSHB bytecount`  
`>SYSCALL fscanf`  
SScanF :  
`>PUSHW s`  
`>PUSHW format`  
`>PUSHW ptr`  
`...`  
`>PUSHB bytecount`  
`>SYSCALL sscanf`  
Specifiers :  
+ %i : short int  
+ %d : byte  
+ %I : int  
+ %D : word  
+ %L : long int  
+ %U : dword  
+ %h : HEX byte  
+ %H : HEX word  
+ %s : string  

TODO : %10s  

## RETURN VALUE  
A = Number of arguments filled.  

# StrToF  
Convert String to 40 bits Float  

## C  
`float strtof (const char* str, char** endptr );`  

## ASM  
**In:**  
`>PUSHW str`  
`>PUSHWI EndPtr`  
`>SYSCALL StrToF`  

## RETURN VALUE  
On stack (float)  

# AToF  
Convert String to 40 bits Float  

## C  
`float atof ( const char* str );`  

## ASM  
**In:**  
`>LDYA str`  
`>SYSCALL atof`  

## RETURN VALUE  
On stack (float)  

# StrToL/StrToUL  
Convert String to 32 bits (unsigned) int  

## C  
`long strtol (const char* str, char** endptr, int base);`  
`unsigned long strtoul (const char* str, char** endptr, int base);`  

## ASM  
**In:**  
`>PUSHW str`  
`>PUSHW EndPtr`  
`>PUSHB Base`  
`>SYSCALL StrToL`  

## RETURN VALUE  
On stack (long)  

# atol  
Convert String to 32 bits long  

## C  
`long atol ( const char * str );`  

## ASM  
**In:**  
`>LDYA str`  
`>SYSCALL atol`  

## RETURN VALUE  
On stack (long)  

# atoi  
Convert String to 16 bits int  

## C  
`int atoi ( const char * str );`  

## ASM  
**In:**  
`>LDYAI str`  
`>SYSCALL atoi`  

## RETURN VALUE  
 Y,A = int  

# RealPath  
Return the canonicalized absolute pathname  

## C / CSH  
`char *realpath(const char *path, char *resolvedpath);`  

## ASM  
**In:**  
`>PUSHW path`  
`>PUSHW resolvedpath`  
`>SYSCALL realpath`  

## RETURN VALUE  
CC : success  
 Y,A = Ptr to Full Path (C-String Buffer, MLI.MAXPATH+1)  
 X = hMem of Full Path  
CS : A = Error Code  

# Expand  

## C  
`char *expand(const char *str, char *expanded);`  

## ASM  
`>PUSHW str`  
`>PUSHW expanded`  
`>SYSCALL expand`  

## RETURN VALUE  
if expanded == null  
 Y,A = PTR to Expanded String  
 X = hMem to Expanded String  
if expanded = null  
 Y,A = strlen  

# StrLen  
Returns Length of C-String  

## C  
`int strlen ( char * str);`  

## ASM  
`>LDYAI str`  
`>SYSCALL strlen`  

## RETURN VALUE   
Y,A = String length  

# StrCat  
Concatenate strings  

## C  
`char * strcat ( char * destination, const char * source );`  

## ASM  
**In:**   
`>PUSHWI destination`  
`>PUSHWI source`  
`>SYSCALL strcat`  

## RETURN VALUE   
Y,A = destination  

# StrCpy  
Copy string  

## C  
`char * strcpy ( char * destination, const char * source );`  

## ASM  
**In:**   
`>PUSHWI destination`  
`>PUSHWI source`  
`>SYSCALL strcpy`  

## RETURN VALUE   
Y,A = destination  

# StrDup  
Create a new copy of this C-String  

## C  
`char * strdup ( char * str);`  

## ASM  
Y,A = Ptr to source C-String  
CC : success   
 Y,A = PTR to String  
 X = hMem (C-String)  
CS : error  
 A = SYS error code  

# StrUpr/StrLwr  
Convert string to UPPERCASE/lowercase  

## C  
`int strupr ( char * str);`  
`int strlwr ( char * str);`  

## ASM  
**In:**   
`>LDYAI str`  
`>SYSCALL strupr`  
`>SYSCALL strlwr`  

## RETURN VALUE   
Uppercased/lowercased String in Buffer  
Y,A = str  

# StrCmp  
Compare 2 strings  

## C  
`int strcmp(const char *s1, const char *s2);`  

## ASM  
**In:**   
`>PUSHWI s1`  
`>PUSHWI s2`  
`>SYSCALL strcmp`  

## RETURN VALUE   
CC : match  
CS : no match  
 CC, Y,A=0  
 CS, Y,A > 0 or < 0  

# StrCaseCmp  
Compare 2 strings, ignoring case  

## C  
`int strcasecmp(const char *s1, const char *s2);`  

## ASM  
**In:**   
`>PUSHWI s1`  
`>PUSHWI s2`  
`>SYSCALL strcasecmp`  

## RETURN VALUE   
CC : match  
CS : no match  
 CC, Y,A=0  
 CS, Y,A > 0 or < 0  

# StrVSet  

## ASM  
`>PUSHB hSTRV`  
`>PUSHW id`  
`>PUSHW ptr`  
`>SYSCALL StrVSet`  

## RETURN VALUE  

# StrVGet  

## ASM  
`>PUSHB hSTRV`  
`>PUSHW id`  
`>PUSHW ptr`  
`>SYSCALL StrVGet`  

## RETURN VALUE  
 CC: Y,A = Ptr  
 CS: Y,A = NULL  

# StrVNew  

## ASM  
`>LDYA size`  
`>SYSCALL StrVNew`  

## RETURN VALUE  

# StrVFree  

## ASM  
`lda hSTRV`  
`>SYSCALL StrVFree`  

## RETURN VALUE  

# Time  
Get System Time in Buffer  

## C  
`int time (S.TIME* timer);`  

## ASM  
`>PUSHW timer`  
`>SYSCALL time`  

## RETURN VALUE  
S.TIME filled with System date/time  

# StrFTime  

## C  
Convert S.TIME struct to CSTR  
`void strftime (char* str, const char* format, const struct S.TIME* timeptr );`  

## ASM  
`>PUSHW str`  
`>PUSHW format`  
+ %a : Abbreviated weekday name : Thu  
+ %A : Full weekday name : Thursday   
+ %b : Abbreviated month name : Aug   
+ %B : Full month name : August   
+ %d : Day of the month, zero-padded (01-31)  
+ %H : Hour in 24h format (00-23) 14   
+ %I : Hour in 12h format (01-12) 02   
+ %m : Month as a decimal number (01-12) 08   
+ %M : Minute (00-59) 55   
+ %p : AM or PM designation PM   
+ %S : Second (00-61) 02   
+ %w : Weekday as a decimal number with Sunday as 0 (0-6)   
+ %y : Year, last two digits (00-99)  
+ %Y : Year four digits 2001   

`>PUSHW timeptr`  
`>SYSCALL strftime`  

## RETURN VALUE  
  none. always succeed.  

# PTime2Time  
 Convert ProDOS Time To S.TIME  

## C  
`int PTime2Time (long* ptime, S.TIME* timer);`  

## ASM  
`>PUSHW ptime`  
`>PUSHW timer`  
`>SYSCALL PTime2Time`  

## RETURN VALUE  

# CTime2Time  
 Convert CTime Time To S.TIME  

## C  
`int CTime2Time (long* ctime, S.TIME* timer);`  

## ASM  
`>PUSHW ctime`  
`>PUSHW timer`  
`>SYSCALL CTime2Time`  

## RETURN VALUE  

# open  

## C  
`hFD open(const char *pathname, short int flags);`  

## ASM  
**In:**  
`>PUSHW pathname`  
`>PUSHB flags`  
`>SYSCALL open`  

## RETURN VALUE  
A = hFD  
REG File created on ProDOS : T=TXT,X=$0000  

# close  

## C  
`int close(hFD fd);`  

## ASM  
**In:**  
`lda fd`  
`>SYSCALL close`  

# read  

## C  
`int read(hFD fd, void *buf, int count);`  

## ASM  
**In:**  
`>PUSHB fd`  
`>PUSHW buf`  
`>PUSHW count`  
`>SYSCALL read`  

## RETURN VALUE  
CC: Y,A = bytes read  
CS: A = EC  

# write  

## C  
`int write(hFD fd, const void *buf, int count);`  

## ASM  
**In:**  
`>PUSHB fd`  
`>PUSHW buf`  
`>PUSHW count`  
`>SYSCALL write`  

## RETURN VALUE  
CC: Y,A = bytes written  
CS: A = EC  

# LSeek  
Set the file-position indicator for hFD  

## C  
`int lseek( short int hFD, long offset, short int whence );`  

## ASM  
**In:**  
`>PUSHB hFD`  
`>PUSHL offset`  
`>PUSHB whence`  
`>SYSCALL fseek`  

# ChOwn  

## C  
 `short int chown(const char *pathname, short int owner, short int group);`  

## ASM  
**In:**  
`>PUSHW pathname`  
`>PUSHB owner`  
`>PUSHB group`  
`>SYSCALL chown`  

## RETURN VALUE  

## License
A2osX is licensed under the GNU General Public License.

    This program is free software; you can redistribute it and/or modify
    it under the terms of the GNU General Public License as published by
    the Free Software Foundation; either version 2 of the License, or
    (at your option) any later version.

    This program is distributed in the hope that it will be useful,
    but WITHOUT ANY WARRANTY; without even the implied warranty of
    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
    GNU General Public License for more details.

The full A2osX license can be found **[Here](../LICENSE)**.

*** End of Auto generated file ***  
