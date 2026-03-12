**VSCODE2022**
- 先完成SDK环境配置
- 测试
    -  ![[Pasted image 20250910154629.png]]
     - ![[Pasted image 20250910154606.png]]
     - 测试程序运行成功，SDK环境配置成功
**进程创建基本知识**
- 进程的概念
     - 在操作系统中，进程（Process） 是程序的⼀次执⾏实例，是系统资源分配和调度的基本单位。每个进 程都有独⽴的内存空间、代码、数据和系统资源（如⽂件句柄、线程等）。进程创建与控制是系统编程 的核⼼功能之⼀
     - 运行一个记事本程序时，系统创建一个进程，可在任务管理器中查看
- 进程创建
     - CreatProcess函数
     - ![[Pasted image 20250910155137.png]]
     - 调用CreatProcess函数的代码示例

```
#include<windows.h>
#include<tchar.h>
#include<stdio.h>

int _tmian()
{
	STARTUPINFO si = { 0 };
	si.cb = sizeof(si);
	PROCESS_INFORMATION pi = { 0 };

	TCHAR szCommandLine[] = _T("notepad.exe");

	if (!CreateProcess(
		NULL,
		szCommandLine,
		NULL, NULL, FALSE, 0, NULL, NULL, &si, &pi))
	{
		_tprintf(_T("CreateProcess faild (%d)\n"), GetLastError());
		return 1;
	}

	CloseHandle(pi.hThread);
	CloseHandle(pi.hProcess);

	return 0;
}
```
-   、
     - **参数IpApplicationName**:⼀个字符串，即将启动的exe程序路径。可以是完整路径和文件名，也可以是部分。最好传递全路径
     - **参数IpCommandLine**:指向一包含命令行字符串地缓冲区。该字符串包含可执行文件名和左右命令行参数，如果IpApplicationName！=NULL,则它要仅包含参数列表
         **实验**：
         - 子进程![[Pasted image 20250910163040.png]]
         - 编写主进程来启动子进程
         - ![[Pasted image 20250910180815.png]]
         - ![[Pasted image 20250910180747.png]]
         - 运行成功，很玄学
    - **参数IpProcessAttributes**:指向 SECURITY_ATTRIBUTES 结构的指针，该结构确定新进程对象的返回句柄是否可以由⼦进程继 承。如果 lpProcessAttributesNULL，则⽆法继承句柄
     - **参数IpThreadAttributes**:指向 SECURITY_ATTRIBUTES 结构的指针，该结构确定新线程对象的返回句柄是否可以由⼦进程继 承。如果 lpThreadAttributesNULL，则⽆法继承句柄
     - bInheritHandles
     - dwCreationFlags
         - 实验：⽤dwCreationFlags函数创建⼀个⾼优先级且在新控制台中运⾏的进程
         - ![[Pasted image 20250910194448.png]]
     - IpEnvironment
     - lpCurrentDirectory
     - lpStartupInfo
     - lpProcessInformation
         - ![[Pasted image 20250910202233.png]]
         - Notepad3
```
#include <windows.h>
#include <tchar.h>
#include <iostream>
#include <string>
#include <sstream>
#include <stdio.h>

// 函数声明
void PrintProcessInfo(DWORD processId, DWORD threadId);
bool IsProcessRunning(DWORD processId);
bool VerifyProcessInTaskManager(DWORD processId);
void WaitForProcessToStabilize(DWORD processId);

int main()
{
    std::cout << "=== 进程创建与信息打印程序 ===" << std::endl;
    std::cout << "正在创建记事本进程..." << std::endl;

    STARTUPINFO si = { 0 };
    PROCESS_INFORMATION pi = { 0 };
    si.cb = sizeof(si);

    wchar_t commandLine[] = L"notepad.exe";

    BOOL result = CreateProcessW(
        NULL,
        commandLine,
        NULL,
        NULL,
        FALSE,
        CREATE_NEW_CONSOLE,
        NULL,
        NULL,
        &si,
        &pi
    );

    if (!result)
    {
        DWORD error = GetLastError();
        std::cout << "CreateProcess 失败 (错误代码: " << error << ")" << std::endl;
        return 1;
    }

    std::cout << "====================================" << std::endl;
    std::cout << "进程创建成功！" << std::endl;
    std::cout << "进程ID (PID): " << pi.dwProcessId << std::endl;
    std::cout << "线程ID (TID): " << pi.dwThreadId << std::endl;
    std::cout << "====================================" << std::endl;

    WaitForProcessToStabilize(pi.dwProcessId);

    std::cout << "验证进程状态..." << std::endl;
    if (IsProcessRunning(pi.dwProcessId))
    {
        std::cout << "进程正在运行中" << std::endl;

        if (VerifyProcessInTaskManager(pi.dwProcessId))
        {
            std::cout << "进程在任务管理器中验证成功!" << std::endl;
        }
        else
        {
            std::cout << "警告: 在任务管理器中未找到该PID的进程!" << std::endl;
        }
    }
    else
    {
        std::cout << "进程已终止" << std::endl;
    }

    PrintProcessInfo(pi.dwProcessId, pi.dwThreadId);

    std::cout << "\n额外验证方法:" << std::endl;
    std::cout << "1. tasklist /fi \"pid eq " << pi.dwProcessId << "\"" << std::endl;
    std::cout << "2. wmic process where processid=" << pi.dwProcessId << " get caption,processid" << std::endl;

    std::cout << "\n按任意键关闭记事本并退出程序..." << std::endl;
    system("pause > nul");

    TerminateProcess(pi.hProcess, 0);
    CloseHandle(pi.hThread);
    CloseHandle(pi.hProcess);

    return 0;
}

// 等待进程稳定运行
void WaitForProcessToStabilize(DWORD processId)
{
    std::cout << "等待进程稳定..." << std::endl;
    for (int i = 0; i < 10; i++)
    {
        if (IsProcessRunning(processId))
        {
            std::cout << "进程已稳定运行" << std::endl;
            return;
        }
        Sleep(500);
    }
    std::cout << "警告: 进程可能未正确启动或已退出" << std::endl;
}

// 检查进程是否正在运行
bool IsProcessRunning(DWORD processId)
{
    HANDLE hProcess = OpenProcess(PROCESS_QUERY_INFORMATION, FALSE, processId);
    if (!hProcess)
        return false;

    DWORD exitCode = 0;
    if (!GetExitCodeProcess(hProcess, &exitCode))
    {
        CloseHandle(hProcess);
        return false;
    }

    CloseHandle(hProcess);
    return (exitCode == STILL_ACTIVE);
}

// 在任务管理器中验证进程
bool VerifyProcessInTaskManager(DWORD processId)
{
    std::string command = "tasklist /fo csv /fi \"pid eq " + std::to_string(processId) + "\"";
    FILE* pipe = _popen(command.c_str(), "r");
    if (!pipe) return false;

    char buffer[256];
    bool found = false;

    // 读取输出 CSV
    while (fgets(buffer, sizeof(buffer), pipe))
    {
        std::string line(buffer);
        if (line.find(std::to_string(processId)) != std::string::npos)
        {
            found = true;
            break;
        }
    }

    _pclose(pipe);
    return found;
}

// 打印进程详细信息
void PrintProcessInfo(DWORD processId, DWORD threadId)
{
    HANDLE hProcess = OpenProcess(PROCESS_QUERY_INFORMATION, FALSE, processId);
    if (!hProcess)
    {
        std::cout << "无法打开进程获取详细信息 (错误: " << GetLastError() << ")" << std::endl;
        return;
    }

    DWORD priority = GetPriorityClass(hProcess);
    std::string priorityStr;
    switch (priority)
    {
    case HIGH_PRIORITY_CLASS: priorityStr = "高"; break;
    case NORMAL_PRIORITY_CLASS: priorityStr = "正常"; break;
    case IDLE_PRIORITY_CLASS: priorityStr = "空闲"; break;
    case REALTIME_PRIORITY_CLASS: priorityStr = "实时"; break;
    case BELOW_NORMAL_PRIORITY_CLASS: priorityStr = "低于正常"; break;
    case ABOVE_NORMAL_PRIORITY_CLASS: priorityStr = "高于正常"; break;
    default: priorityStr = "未知"; break;
    }

    std::cout << "进程优先级: " << priorityStr << std::endl;

    DWORD exitCode;
    if (GetExitCodeProcess(hProcess, &exitCode))
    {
        if (exitCode == STILL_ACTIVE)
            std::cout << "进程状态: 运行中" << std::endl;
        else
            std::cout << "进程状态: 已退出 (退出代码: " << exitCode << ")" << std::endl;
    }

    CloseHandle(hProcess);
}

```
- 1 
     - ![[Pasted image 20250910203054.png]]
     - 验证线程，对的上
- 其它进程创建函数
     - CreateProcess
     - NTCreateProcess
     - ShellExecute
**进程控制**
- 主要函数介绍
     - Openprocess:打开⼀个已存在的进程，获取其句柄，从⽽对它进⾏后续控制（如终⽌、读写内存等）
         ![[Pasted image 20250910210704.png]]
         "D:\VS2022PROJECT\huoqujubin"
     - TerminateProcess:强制终止指定进程
     - WaitForSingleObject：等待进程结束
     - GetExitCodeProcess：获得进程退出代码
     - CloseHandle：关闭句柄
     - GetCurrentProcessId：获取当前进程ID
     - GetCurrentProcess：获取当前进程句柄
- 更多进程控制API
- https://learn.microsoft.com/en-us/windows/win32/apiindex/windows-api-list


# 其它
-  句柄 
     - 句柄就是**系统为了方便管理某个资源（比如窗口、文件、进程等）而分配给你的一个唯一编号或引用，你通过它来间接操作资源，而不是直接操作资源本身**。
- 