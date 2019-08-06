
## Windows API

```cs
[DllImport("Kernel32.dll", SetLastError = true)]
static extern IntPtr CreateEvent(IntPtr lpEventAttributes, bool bManualReset, bool bInitialState, string lpName);
[DllImport("Kernel32.dll", SetLastError = true)]
static extern IntPtr OpenEvent(uint dwDesiredAccess, bool bInheritHandle, string lpName);
[DllImport("Kernel32.dll")]
static extern bool SetEvent(IntPtr hEvent);
[DllImport("Kernel32.dll")]
static extern WaitForSingleObjectResult WaitForSingleObject(IntPtr hHandle, int dwMilliseconds);
```

## 触发 Windows 事件

```cs
/// <summary>
/// 触发 Windows 事件
/// </summary>
/// <param name="lpName">事件的命名管道</param>
IntPtr TriggerEvent(string lpName)
{
    IntPtr handle = OpenEvent((uint)FILE_MAP_WRITE, false, lpName);
    if (handle != IntPtr.Zero)
    {
        SetEvent(handle);
    }
    else
    {
        handle = CreateEvent(IntPtr.Zero, false, false, lpName);
        if (handle != IntPtr.Zero)
        {
            SetEvent(handle);
        }
    }

    return handle;
}
```

## 等待 Windows 事件

```cs
static WaitForSingleObjectResult WaitEvent(string lpName)
{
    IntPtr handle = CreateEvent(IntPtr.Zero, false, false, lpName);
    return IntPtr.Zero == handle ? WaitForSingleObjectResult.WAIT_FAILED : WaitForSingleObject(handle, 15000);
}
```


## 相关项
```cs
[Flags]
enum WaitForSingleObjectResult : long
{
    WAIT_ABANDONED = 0x00000080L,
    WAIT_OBJECT_0 = 0x00000000L,
    WAIT_TIMEOUT = 0x00000102L,
    WAIT_FAILED = 0xFFFFFFFF
}
```

```cs
const int ERROR_ALREADY_EXISTS     = 183;

const int FILE_MAP_COPY            = 0x0001;
const int FILE_MAP_WRITE           = 0x0002;
const int FILE_MAP_READ            = 0x0004;
const int FILE_MAP_ALL_ACCESS      = 0x0002 | 0x0004;

const int PAGE_READONLY            = 0x02;
const int PAGE_READWRITE           = 0x04;
const int PAGE_WRITECOPY           = 0x08;
const int PAGE_EXECUTE             = 0x10;
const int PAGE_EXECUTE_READ        = 0x20;
const int PAGE_EXECUTE_READWRITE   = 0x40;

const int SEC_COMMIT               = 0x8000000;
const int SEC_IMAGE                = 0x1000000;
const int SEC_NOCACHE              = 0x10000000;
const int SEC_RESERVE              = 0x4000000;

const int INVALID_HANDLE_VALUE     = -1;
```
