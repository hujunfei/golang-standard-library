##package os

<code>import "os"</code>

包<code>os</code>提供了一个不依赖于具体平台操作系统函数的接口。设计是类Unix的，而错误处理是类Go形式的；失败调用返回error类型而不是错误码。通常来说，在返回值error中可以包含更多的信息。例如，如果一个带有文件名的函数调用失败，如<code>Open</code>或<code>Stat</code>，error将会是*PathError类型，并包含失败的文件名，同时包含更多其他信息。

os接口本意是用来统一所有操作系统。包中没有的特性可以在特定于操作系统<code>syscall</code>包中找到。

以下是一个简单的例子：打开一个文件并读取。

```go
file, err := os.Open("file.go") // For read access.
if err != nil {
	log.Fatal(err)
}
```

如果打开失败，error字符串是自解释的，类似于：

`
open file.go: no such file or directory
`

文件中的数据将会读入字节切片(slice)中。<code>Read</code>和<code>Write</code>从参数切片的长度中获取字节计数(？*这一句不知道怎么翻译。注：指定字节切片长度，读入和写出时返回实际操作长度，但不超过原切片长度*)

```go
data := make([]byte, 100)
count, err := file.Read(data)
if err != nil {
	log.Fatal(err)
}
fmt.Printf("read %d bytes: %q\n", count, data[:count])
```

##Index##

    Constants
    Variables
    func Chdir(dir string) error
    func Chmod(name string, mode FileMode) error
    func Chown(name string, uid, gid int) error
    func Chtimes(name string, atime time.Time, mtime time.Time) error
    func Clearenv()
    func Environ() []string
    func Exit(code int)
    func Expand(s string, mapping func(string) string) string
    func ExpandEnv(s string) string
    func Getegid() int
    func Getenv(key string) string
    func Geteuid() int
    func Getgid() int
    func Getgroups() ([]int, error)
    func Getpagesize() int
    func Getpid() int
    func Getppid() int
    func Getuid() int
    func Getwd() (pwd string, err error)
    func Hostname() (name string, err error)
    func IsExist(err error) bool
    func IsNotExist(err error) bool
    func IsPathSeparator(c uint8) bool
    func IsPermission(err error) bool
    func Lchown(name string, uid, gid int) error
    func Link(oldname, newname string) error
    func Mkdir(name string, perm FileMode) error
    func MkdirAll(path string, perm FileMode) error
    func NewSyscallError(syscall string, err error) error
    func Readlink(name string) (string, error)
    func Remove(name string) error
    func RemoveAll(path string) error
    func Rename(oldname, newname string) error
    func SameFile(fi1, fi2 FileInfo) bool
    func Setenv(key, value string) error
    func Symlink(oldname, newname string) error
    func TempDir() string
    func Truncate(name string, size int64) error
    type File
        func Create(name string) (file *File, err error)
        func NewFile(fd uintptr, name string) *File
        func Open(name string) (file *File, err error)
        func OpenFile(name string, flag int, perm FileMode) (file *File, err error)
        func Pipe() (r *File, w *File, err error)
        func (f *File) Chdir() error
        func (f *File) Chmod(mode FileMode) error
        func (f *File) Chown(uid, gid int) error
        func (f *File) Close() error
        func (f *File) Fd() uintptr
        func (f *File) Name() string
        func (f *File) Read(b []byte) (n int, err error)
        func (f *File) ReadAt(b []byte, off int64) (n int, err error)
        func (f *File) Readdir(n int) (fi []FileInfo, err error)
        func (f *File) Readdirnames(n int) (names []string, err error)
        func (f *File) Seek(offset int64, whence int) (ret int64, err error)
        func (f *File) Stat() (fi FileInfo, err error)
        func (f *File) Sync() (err error)
        func (f *File) Truncate(size int64) error
        func (f *File) Write(b []byte) (n int, err error)
        func (f *File) WriteAt(b []byte, off int64) (n int, err error)
        func (f *File) WriteString(s string) (ret int, err error)
    type FileInfo
        func Lstat(name string) (fi FileInfo, err error)
        func Stat(name string) (fi FileInfo, err error)
    type FileMode
        func (m FileMode) IsDir() bool
        func (m FileMode) IsRegular() bool
        func (m FileMode) Perm() FileMode
        func (m FileMode) String() string
    type LinkError
        func (e *LinkError) Error() string
    type PathError
        func (e *PathError) Error() string
    type ProcAttr
    type Process
        func FindProcess(pid int) (p *Process, err error)
        func StartProcess(name string, argv []string, attr *ProcAttr) (*Process, error)
        func (p *Process) Kill() error
        func (p *Process) Release() error
        func (p *Process) Signal(sig Signal) error
        func (p *Process) Wait() (*ProcessState, error)
    type ProcessState
        func (p *ProcessState) Exited() bool
        func (p *ProcessState) Pid() int
        func (p *ProcessState) String() string
        func (p *ProcessState) Success() bool
        func (p *ProcessState) Sys() interface{}
        func (p *ProcessState) SysUsage() interface{}
        func (p *ProcessState) SystemTime() time.Duration
        func (p *ProcessState) UserTime() time.Duration
    type Signal
    type SyscallError
        func (e *SyscallError) Error() string

##Package Files##

dir_unix.go doc.go env.go error.go error_unix.go exec.go exec_posix.go exec_unix.go file.go file_posix.go file_unix.go getwd.go path.go path_unix.go pipe_linux.go proc.go stat_linux.go sys_linux.go types.go types_notwin.go 

##常量(Constants)
```go
const (
    O_RDONLY int = syscall.O_RDONLY // 只读方式打开文件
    O_WRONLY int = syscall.O_WRONLY // 只写方式打开文件
    O_RDWR   int = syscall.O_RDWR   // 读写方式打开文件
    O_APPEND int = syscall.O_APPEND // 若写文件，则为追加方式
    O_CREATE int = syscall.O_CREAT  // 如果不存在，则创建新文件
    O_EXCL   int = syscall.O_EXCL   // 和O_CREATE一起使用, 文件必须不存在
    O_SYNC   int = syscall.O_SYNC   // 同步I/O方式打开
    O_TRUNC  int = syscall.O_TRUNC  // 如果可能，打开的时候清空文件
)
```
<code>Open</code>的标志值包装了底层系统的标志值。但某些特定的系统可能不会实现所有的标志值。
```go
const (
    SEEK_SET int = 0 // 相对于文件起始查找
    SEEK_CUR int = 1 // 相对于当前位移查找
    SEEK_END int = 2 // 相对于文件末尾查找
)
```
以上为<code>Seek</code>从何处查找的标志值
```go
const (
    PathSeparator     = '/' // 特定于OS的路径分隔符
    PathListSeparator = ':' // 特定于OS的路径列表分隔符
)
```
```go
const DevNull = "/dev/null"
```
<code>DevNull</code>是操作系统中"null设备"的名称。类Unix中为"/dev/null"；Windows中为"NUL"。

##变量(Variables)
```go
var (
    ErrInvalid    = errors.New("invalid argument")
    ErrPermission = errors.New("permission denied")
    ErrExist      = errors.New("file already exists")
    ErrNotExist   = errors.New("file does not exist")
)
```
一些通用系统调用错误的可移植衍生对象
```go
var (
    Stdin  = NewFile(uintptr(syscall.Stdin), "/dev/stdin")
    Stdout = NewFile(uintptr(syscall.Stdout), "/dev/stdout")
    Stderr = NewFile(uintptr(syscall.Stderr), "/dev/stderr")
)
```
<code>Stdin</code>、<code>Stdout</code>和<code>Stderr</code>为指向已打开的标准输入、标准输出和标准错误文件描述符的文件指针。
```go
var Args []string
```
<code>Args</code>包含命令行参数，起始值为程序名称。

##函数

###func Chdir
```go
func Chdir(dir string) error
```
<code>Chdir</code>设置当前工作目录为指定目录。若产生error，则类型为<code>*PathError</code>。
###func Chmod
```go
func Chmod(name string, mode FileMode) error
```
<code>Chmod</code>修改指定目录的文件模式(mode)。若文件是软链接，则修改链接指向的目标文件的模式。若产生error，则类型为<code>*PathError</code>。
###func Chown
```go
func Chown(name string, uid, gid int) error
```
<code>Chown</code>修改指定文件的uid和gid。若文件是软链接，则修改链接指向的目标文件的uid和gid。若产生error，则类型为<code>*PathError</code>。
###func Chtimes
```go
func Chtimes(name string, atime time.Time, mtime time.Time) error
```
<code>Chtimes</code>修改指定文件的访问和修改时间，类似于Unix的<code>utime()</code>或<code>utimes()</code>函数
底层文件系统可能会将时间截短或者舍入为精确度稍低一些的时间单位。若产生error，则类型为<code>*PathError</code>。
###func Clearenv
```go
func Clearenv()
```
<code>Clearenv</code>删除所有环境变量
###func Environ()
```go
func Environ() []string
```
<code>Environ</code>以"key=value"的形式返回环境变量字符串的拷贝
###func Exit()
```go
func Exit(code int)
```
<code>Exit</code>导致当前程序以指定状态码退出。按照惯例，0表示成功，非0值表示错误。程序立即终止，deferred函数不再执行
###func Expand
```go
func Expand(s string, mapping func(string) string) string
```
<code>Expand</code>使用映射函数替换字符串中${var}或$var。例如，os.ExpandEnv(s)相当于os.Expand(s, os.Getenv)。
###func ExpandEnv
```go
func ExpandEnv(s string) string
```
<code>ExpandEnv</code>使用当前环境变量值替换字符串中${var}或$var。未定义的变量引用替换为空值。
###func Getegid
```go
func Getegid() int
```
<code>Getegid</code>返回调用者的有效组ID，即egid
###func Getenv
```go
func Getenv(key string) string
```
<code>Getenv</code>获取指定键的环境变量值。如果该环境变量不存在，则返回空串""。
###func Geteuid
```go
func Geteuid() int
```
<code>Geteuid</code>返回调用者的有效用户ID，即euid
###func Getgid
```go
func Getgid() int
```
<code>Getgid</code>返回调用者的组ID，即gid
###func Getgroups
```go
func Getgroups() ([]int, error)
```
<code>Getgroups</code>返回调用者所属组ID的列表