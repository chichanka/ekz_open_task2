#include <windows.h>
#include <iostream>
#include <fstream>

const wchar_t* MUTEX_NAME = L"SharedMutex";
const wchar_t* SHARED_FILE = L"shared_data.txt";

void writeToFile(const std::wstring& data) {
    std::wofstream outFile(SHARED_FILE, std::ios::out | std::ios::trunc);
    if (outFile.is_open()) {
        outFile << data;
        outFile.close();
    }
}

int main() {
    HANDLE hMutex = CreateMutexW(NULL, FALSE, MUTEX_NAME);
    if (hMutex == NULL) {
        return 1;
    }

std::wstring sharedData = L"Hello from the first process!";
    writeToFile(sharedData);

WaitForSingleObject(hMutex, INFINITE);

CloseHandle(hMutex);
    return 0;
}
