# Offsets

*NOTE: Needs to be cleaned*

*All offsets are for GOG and for SKIDROW crack*

**gamiee:**
```
0x754040 - probably function to define functions in native Lua functions
0x753B20 - require lua func
0x9CCF30 - WSLoadDisplayManager::Update_Display - Renders main menu 
0x429700 - WndProc
0x120F461 - Global Variable, bool, 1 => window is focused, 0 => not focused
0x00429785 - This sets focus variable to zero, nop it to not open settings when window is unfocused
```

**Dan:**
```cpp
// Camera mode settings:
// 00000000: Normal (Follows player)
// 00000001: Scenic (Controlled programmatically, e.g. scenic view animation & main menu fixed location)
// 00000002: Static (Stationary, does not follow player)

*(DWORD *)(*(DWORD *)0x1321b74 + 0x2c) = cameraMode;
```

**Igoh:**

all together should remove all the peds on the map
```cpp
    Unprotect(0x9cccac, 1);
    *(BYTE *)0x9cccac = 0xeb;

    Unprotect(0x461eb1, 1);
    *(BYTE *)0x461eb1 = 0xeb; // "aispawner"
    Unprotect(0x6f5d6c, 1);
    *(BYTE *)0x6f5d6c = 0xeb; // disable res spawner
    Unprotect(0x6f5de9, 1);
    *(BYTE *)0x6f5de9 = 0xeb; // disable nazi spawner
    Unprotect(0x907816, 6);
    *(DWORD *)0x907816 = 393705;
    *(BYTE *)(0x907816 + 5) = 0x90; // disable patrol spawner

    Unprotect(0x9909D3, 2);
    *(__int16 *)0x9909D3 = 12523;

    Unprotect(0x990AC5, 2);
    *(__int16 *)0x990AC5 = 12779;

    Unprotect(0x99099B, 2);
    *(__int16 *)0x99099B = 14059;

    Unprotect(0x99096f, 2);
    *(__int16 *)0x99096f = 10987;

    // Disposables_MaxMale | Disposables_MaxFemale | Disposables_MaxVendor | Disposables_NaziPadding
    Unprotect(0x81433A, 2);
    *(unsigned __int16 *)0x81433A = 32491;

    Unprotect(0x8143B1, 4);
    *(DWORD *)0x8143B1 = 0xff6914;

    // Remove spores
    Unprotect(0x49574f, 6);
    memcpy((LPVOID)0x49574F, (void *)"\xe9\x8f\x08\x00\x00\x90", 6);
    Unprotect(0x461AC5, 1);
    *(BYTE *)0x461AC5 = 0xeb;
```
ped pool
```cpp
    DWORD *pedPool = *(DWORD **)(*(DWORD *)0x14a9ce0 + 0x34);
    for (; pedPool; pedPool = *(DWORD**)pedPool)
    {
        DWORD _ped = pedPool[2];
        if (_ped == NULL)
            break;

        DWORD _baseBegin = *(DWORD *)(_ped + 0x950 + 0x150);

        // probably a matrix or some shit
        DWORD _baseTwo = *(DWORD *)(_baseBegin + 0x30);
        DWORD _baseT = *(DWORD *)(_baseTwo + 0x18);
        DWORD _baseR = _baseT + 0x30;
    
        float x = *(float *)(_baseR + 0);
        float y = *(float *)(_baseR + 4);
        float z = *(float *)(_baseR + 8);
        float rot = *(float *)(*(DWORD *)(_ped + 0x10CC) + 0x704);

        pChatWindow->AddDebugMessage("Ped: 0x%p (bb: 0x%p, b: 0x%p), pos: %f %f %f %f", _ped, _baseBegin, _baseR, x, y, z, rot);
    }
```
```cpp
void CGame::ShowMinimap(BYTE bToggle)
{
    *(BYTE *)(*(DWORD *)(*(DWORD *)0x14a9b14 + 0x8c) + 0x35) = bToggle;
}
```
```cpp
unsigned __int16 CGame::GetTime()
{
    float actualTime = *(float *)(*(DWORD *)0x14941EC + 0x18);
    BYTE result[2] = { 0, 0 };

    int bMinutes = ((int)actualTime % 3600) / 60;
    int bHours = (int)actualTime / 3600;
    result[0] = bHours;
    result[1] = bMinutes;

    return *(unsigned __int16 *)result;
}
```
```cpp
void CGame::SetTime(int bHour, int bMinute)
{
    if(bHour >= 24)
        bHour = 23;

    if(bMinute > 59)
        bMinute = 59;

    float time = bMinute * 60.0 + bHour * 3600.0;

    *(float *)(*(DWORD *)0x14941EC + 0x18) = time;
    *(float *)(*(DWORD *)0x14941EC + 0x40) = time;
}
```
```cpp
void CGame::SetWTFMode(BOOL bToggle)
{
    *(DWORD *)(*(DWORD *)0x147dcac + 0x84) = 2863268097;
    *(DWORD *)(*(DWORD *)0x147dcac + 0x9c) = 2863311361;
    *(DWORD *)(*(DWORD *)0x147dcac + 0xac) = 1084227584;
    *(DWORD *)(*(DWORD *)0x147dcac + 0x90) = bToggle ? 2863311360 : 2863311361;
}
```
current HP:
```cpp
*(float *)(*(DWORD *)0x123f8b8 + 0x3fc)
```
```cpp
void CGame::FreezeTime(BOOL bToggle)
{
    *(float *)(*(DWORD *)0x14941EC + 0x1c) = bToggle ? 0 : 12;
}
```
```cpp
void CGame::SetCameraMatrix(MATRIX4X4 matPos)
{
    *(__int16 *)0x444F40 = 6379;
    *(__int16 *)0x004376C0 = 25323;

    DWORD cameraMatrix = *(DWORD *)0x14aacac + 0xb0;
    matPos.pad_p = 1.0f;

    memcpy((LPVOID)cameraMatrix, &matPos, sizeof(MATRIX4X4));

    *(__int16 *)0x444F40 = 17547;
    *(__int16 *)0x004376C0 = -15989;
}
```
mainwnd:
```cpp
*(HWND *)0x120F458
```
is paused vel in menu:
```cpp
*(DWORD *)0x14d6d38 == 0x200;
```
```cpp
// Disable global suspicion
Unprotect(0x11bb674, 1);
*(BYTE *)0x11bb674 = 0;
Unprotect(0x45DCBD, 1);
*(BYTE *)0x45DCBD = 0;
```
```cpp
// Install OnDeath hook
InstallJmpHook(0x5B7DEB, (DWORD)OnDeath);

...

DWORD dwJmp = 0;
void __declspec(naked) OnDeath()
{
    dwJmp = 0x5B7E8A;
    
    printf("im in death");

    *(DWORD *)0x111c334 = 1120403456; // prevents calling this func in loop

    _asm
    {
        jmp dwJmp
    }
}
```
```cpp
// fade in, last argument is float how long it will take to fade the screen
DWORD dwColor = 255 | ((255 | ((255 | (255 << 8)) << 8)) << 8);
((void(__fastcall*)(DWORD, DWORD, DWORD, float))0x9bc170)(*(DWORD *)0x14A9B14, 0, dwColor, 0);
```
```cpp
// sets bird density
*(BYTE *)(*(DWORD *)0x12102d0 + 0x67e4) = 254;
```
```cpp
// togle birds
*(BYTE *)(*(DWORD *)0x12102d0 + 0x67e5) = bToggle ? 168 : 160;
```
```cpp
// toggle mini zep
*(BYTE *)(*(DWORD *)0x143d958 + 0xec) = bToggle ? 166 : 160;
```
```cpp
// sets drunk level
*(float *)(*(DWORD *)0x143d02c + 0x28) = fDrunk;
```
```cpp
// force camera behind player, 167 disables moving camera
*(BYTE *)(*(DWORD *)0x1494360 + 0x10c60) = 168;
```
```cpp
// game speed
*(float *)0x14e1c6c = 1000;
```
```cpp
// death state
*(BYTE *)(*(DWORD *)0x123f8b8 + 0xa98) = 0;
```
```cpp
// disables falling sounds
jmp at 0x54EFE1
```
```cpp
0x9E8D20 // func which displays stats menu, has all struct offsets listed with names
```
```cpp
0x14AADCC - struct pointer

#define _pad(x,y) BYTE x[y]

typedef struct _STATS {
    _pad(__pad0a, 0x18); // 000-024
    DWORD naziWehrmachtKilled;
    DWORD naziKrigsmarineSquadKilled;
    _pad(__pad0b, 0x4); // removed ??
    DWORD naziGestapoKilled;
    DWORD naziSSKilled;
    DWORD naziTerrorSquadKilled;
    DWORD naziDoppelziegKilled;
    _pad(__pad0c, 0x4); // end nazi killed stats
    
    DWORD mostNazisKilledInOneLife;
    _pad(__pad0d, 0x4);
    DWORD mostNazisKilledAtOnce;
    DWORD mostNazisKilledWhileDriving;
    DWORD mostNazisKilledBySuprise;
    _pad(__pad0e, 0x4);
    
    DWORD birdsKilled;
    DWORD civiliansKilledByPlayer;
    DWORD civiliansKilledByNazis;
    DWORD resistanceKilled;
    
    DWORD cigarettesSmoked;
    
    DWORD totalBombsPlanted;
    DWORD civiliansSaved;
    DWORD flamethrowerFuelSpent;
    
    _pad(__pad0f, 0x8);
    
    // and keeps goin
} STATS;
```
```cpp
// prevents menu open 0 enables it bck
*(BYTE *)0x1240545 = 1;
```
```cpp
*(BYTE *)(*(DWORD *)0x120F5C4 + 0x3fdfd) = 1; // disables load & save buttons in menu
// or
*(BYTE *)0x7a4346 = 0xeb; // disables for good
```
```cpp
*(__int16 *)0x5C136B = 16619; // disables quickload from button
```
```cpp
*(BYTE *)0x4365bc = 0xc3; // disables quicksave from button (disable it after character spawned, otherwise game won't get through start loading)
```