# Декомпиляция и настройка C#-проекта

Написание C#-кода для модов RimWorld требует декомпиляции сборки `Assembly-CSharp.dll` для изучения ванильной логики и настройки C#-проекта (`.csproj`).

---

## 1. Декомпиляция сборки игры (`Assembly-CSharp.dll`)

Главная логика RimWorld скомпилирована в файле:
`C:\Program Files (x86)\Steam\steamapps\common\RimWorld\RimWorldWin64_Data\Managed\Assembly-CSharp.dll`

### Инструменты декомпиляции
- **ILSpy / dnSpy**: Откройте `Assembly-CSharp.dll` в декомпиляторе для быстрого поиска классов по названию (`Pawn`, `Building`, `Verb_Shoot`, `JobDriver`).
- **`ilspycmd` (CLI для агентов)**:
  ```bash
  ilspycmd -p -o ./DecompiledSource "C:\Program Files (x86)\Steam\steamapps\common\RimWorld\RimWorldWin64_Data\Managed\Assembly-CSharp.dll"
  ```

---

## 2. Настройка C# проекта (`.csproj`)

Для создания сборки мода (`Assemblies/MyMod.dll`) используется файл `.csproj` под .NET Framework 4.8 или .NET Standard 2.0 (согласно требованиям Unity/RimWorld).

Пример файла `Source/MyMod.csproj`:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>net48</TargetFramework>
    <OutputType>Library</OutputType>
    <RootNamespace>MyMod</RootNamespace>    <AssemblyName>MyModAssembly</AssemblyName>
    <OutputPath>..\Assemblies\</OutputPath>
    <AppendTargetFrameworkToOutputPath>false</AppendTargetFrameworkToOutputPath>
  </PropertyGroup>

  <ItemGroup>
    <!-- Ссылка на базовые сборки RimWorld -->
    <Reference Include="Assembly-CSharp">
      <HintPath>C:\Program Files (x86)\Steam\steamapps\common\RimWorld\RimWorldWin64_Data\Managed\Assembly-CSharp.dll</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="UnityEngine.CoreModule">
      <HintPath>C:\Program Files (x86)\Steam\steamapps\common\RimWorld\RimWorldWin64_Data\Managed\UnityEngine.CoreModule.dll</HintPath>
      <Private>False</Private>
    </Reference>
    <Reference Include="UnityEngine.IMGUIModule">
      <HintPath>C:\Program Files (x86)\Steam\steamapps\common\RimWorld\RimWorldWin64_Data\Managed\UnityEngine.IMGUIModule.dll</HintPath>
      <Private>False</Private>
    </Reference>
  </ItemGroup>

</Project>
```

> [!IMPORTANT]
> **Параметр `<Private>False</Private>`**: Обязателен для всех ссылок на сборки RimWorld и Unity, чтобы не копировать бинарники игры в папку `Assemblies/` вашего мода.

---

## 3. Компиляция сборки

### Использование `dotnet` CLI:
```bash
dotnet build -c Release
```
После завершения компилируемый файл `MyModAssembly.dll` автоматически появится в папке `Assemblies/` вашего мода.
