## Daftar Isi
1. [Metode Pemuatan (Installation)](#1-metode-pemuatan-installation)
2. [Inisialisasi Window Utama](#2-inisialisasi-window-utama)
3. [Manajemen Sidebar (Group & Tab)](#3-manajemen-sidebar-group--tab)
4. [Komponen Kontrol (Elemen Input)](#4-komponen-kontrol-elemen-input)
   - [AddToggle (Saklar)](#1-addtoggle-saklar)
   - [AddSlider (Penggeser Angka)](#2-addslider-penggeser-angka)
   - [AddSpinner (Angka Plus/Minus)](#3-addspinner-angka-plusminus)
   - [AddDropdown (Pilihan Menu)](#4-adddropdown-pilihan-menu)
   - [AddInput (Kotak Teks)](#5-addinput-kotak-teks)
   - [AddColorpicker (Pemilih Warna)](#6-addcolorpicker-pemilih-warna)
   - [AddButton (Tombol Pemicu)](#7-addbutton-tombol-pemicu)
5. [Sistem Notifikasi Pop-up](#5-sistem-notifikasi-pop-up)
6. [Penyimpanan Konfigurasi (Config System)](#6-penyimpanan-konfigurasi-config-system)
7. [Contoh Script Lengkap (Template Developer)](#7-contoh-script-lengkap-template-developer)

---

## 1. Metode Pemuatan (Installation)

--loadstring

```lua
local SysUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/tya1002/SysUI-Library/main/loader?t=" .. os.time()))()
```

---

## 2. Inisialisasi Window Utama

Panggil `:Load()` untuk membuat dan memunculkan panel antarmuka utama. Tombol default untuk menyembunyikan/memunculkan UI adalah **RightShift** (bisa diubah oleh pengguna melalui panel Settings bawaan).

```lua
local Window = SysUI:Load()
```

---

## 3. Manajemen Sidebar (Group & Tab)

Desain sidebar SysUI terbagi menjadi **Group** (kategori besar yang dapat dilipat) dan **Tab** (submenu di dalam kategori tersebut). Slot baris dan halaman diatur secara otomatis.

### Membuat Group Baru
```lua
-- Group collapsible (dapat dilipat saat diklik) - Direkomendasikan
local CombatGroup = Window:CreateGroup("Combat Features", { Collapsible = true })

-- Group statis (selalu terbuka secara permanen)
local InfoGroup = Window:CreateGroup("Information", { Collapsible = false })
```
*Maksimal 3 Group utama pada sidebar.*

### Membuat Tab di dalam Group
```lua
local CombatTab = CombatGroup:CreateTab("Aimbot & Rage")
local MiscTab = CombatGroup:CreateTab("Player Cheats")
```
*Maksimal 8 Tab di dalam setiap Group.*

---

## 4. Komponen Kontrol (Elemen Input)

Setiap elemen ditambahkan ke halaman Tab secara otomatis ke baris kosong berikutnya tanpa perlu melacak indeks baris secara manual.

### 1. AddToggle (Saklar)
Saklar aktif/nonaktif sederhana.
```lua
CombatTab:AddToggle("Aimbot Status", {
    Default = false, -- Status awal
    Keybind = Enum.KeyCode.Q, -- (Opsional) Hotkey untuk toggle
    Callback = function(enabled)
        print("Aimbot aktif:", enabled)
    end
})
```

### 2. AddSlider (Penggeser Angka)
Berguna untuk pengaturan nilai rentang. Bisa ditambahkan toggle & keybind langsung pada baris yang sama.
```lua
CombatTab:AddSlider("Aimbot FOV", {
    Min = 10,
    Max = 800,
    Default = 150,
    Unit = " px", -- Unit di belakang angka
    Toggle = {
        Default = false, -- Status saklar toggle di sebelah slider
        Callback = function(enabled)
            print("Toggle FOV aktif:", enabled)
        end
    },
    Keybind = Enum.KeyCode.H, -- (Opsional) Hotkey toggle
    Callback = function(value)
        print("Nilai FOV diubah ke:", value)
    end
})
```

### 3. AddSpinner (Angka Plus/Minus)
Mengubah nilai angka menggunakan tombol `-` dan `+` secara presisi. Bisa dikombinasikan dengan toggle & keybind.
```lua
MiscTab:AddSpinner("WalkSpeed Val", {
    Min = 16,
    Max = 300,
    Default = 50,
    Step = 5, -- Nilai lompatan per klik
    Toggle = {
        Default = false,
        Callback = function(enabled)
            print("WalkSpeed modifikasi aktif:", enabled)
        end
    },
    Keybind = Enum.KeyCode.X, -- (Opsional) Hotkey toggle
    Callback = function(value)
        print("Nilai WalkSpeed diubah ke:", value)
    end
})
```

### 4. AddDropdown (Pilihan Menu)
Menu pilihan dropdown dengan opsi kustom. Bisa dikombinasikan dengan toggle & keybind.
```lua
CombatTab:AddDropdown("Target Hitbox", {
    Options = {"Head", "Torso", "HumanoidRootPart"},
    Default = "Head",
    Toggle = {
        Default = false,
        Callback = function(enabled)
            print("Kunci target aktif:", enabled)
        end
    },
    Keybind = Enum.KeyCode.Z, -- (Opsional) Hotkey toggle
    Callback = function(selected)
        print("Bagian tubuh terpilih:", selected)
    end
})
```

### 5. AddInput (Kotak Teks)
Kolom input untuk mengetik teks kustom secara langsung.
```lua
MiscTab:AddInput("Custom Announcement", {
    Placeholder = "Ketik sesuatu...", -- Teks petunjuk saat kosong
    Callback = function(text)
        print("Teks yang dimasukkan:", text)
    end
})
```

### 6. AddColorpicker (Pemilih Warna)
Membuka panel palet warna untuk kustomisasi warna secara visual. Menggunakan 2D gradient gradient offline yang sangat cepat dan ringan.
```lua
MiscTab:AddColorpicker("UI Primary Color", {
    Default = Color3.fromRGB(0, 120, 255),
    Callback = function(color)
        print("Warna baru terpilih (RGB):", color)
    end
})
```

### 7. AddButton (Tombol Pemicu)
Tombol kustom untuk memicu fungsi instan.
```lua
MiscTab:AddButton("Teleport ke Spawn", {
    Callback = function()
        print("Melakukan teleportasi...")
    end
})
```

---

## 5. Sistem Notifikasi Pop-up

Tampilkan notifikasi modern di sudut kanan bawah layar game:

```lua
SysUI:Notify({
    Title = "Sistem Aktif",
    Content = "Seluruh cheat berhasil dimuat!",
    Duration = 3 -- Tampil selama 3 detik
})
```

---

## 6. Penyimpanan Konfigurasi (Config System)

Sistem profil konfigurasi disimpan secara otomatis di file `syshub_config.json`. Anda hanya perlu menempelkan hook callback untuk menyinkronkan data Anda saat profil disimpan atau dimuat ulang:

```lua
Window:BindConfigActions({
    OnLoad = function()
        print("Konfigurasi profil berhasil diterapkan!")
    end,
    OnSave = function()
        print("Konfigurasi profil berhasil disimpan!")
    end
})
```
*Opsi Save/Load Config secara visual dapat diakses di bagian tab **Settings** di pojok kiri bawah UI.*

---

## 7. Contoh Script Lengkap (Template Developer)

Berikut adalah contoh script lengkap yang siap digunakan oleh developer lain sebagai template dasar pengembangan script cheat:

```lua
-- 1. Pemuatan Library
local SysUI = loadstring(game:HttpGet("https://raw.githubusercontent.com/tya1002/SysUI-Library/main/loader?t=" .. os.time()))()
local Window = SysUI:Load()

-- 2. Manajemen Menu Sidebar Kiri
local MainCheats = Window:CreateGroup("Cheat Utama", { Collapsible = true })
local Visuals = Window:CreateGroup("Visual & ESP", { Collapsible = true })

local CombatTab = MainCheats:CreateTab("Pertempuran")
local MovementTab = MainCheats:CreateTab("Pergerakan")
local ESPTab = Visuals:CreateTab("ESP Pemain")

-- 3. Inisialisasi Elemen di Tab Pertempuran (Combat)
CombatTab:AddToggle("Aimbot Aktif", {
    Default = false,
    Keybind = Enum.KeyCode.E,
    Callback = function(enabled)
        print("Aimbot state:", enabled)
    end
})

CombatTab:AddSlider("Rentang FOV", {
    Min = 50,
    Max = 800,
    Default = 150,
    Unit = " px",
    Callback = function(val)
        print("FOV Range:", val)
    end
})

CombatTab:AddDropdown("Target Bagian Tubuh", {
    Options = {"Head", "Torso", "HumanoidRootPart"},
    Default = "Head",
    Callback = function(part)
        print("Hitbox target set:", part)
    end
})

-- 4. Inisialisasi Elemen di Tab Pergerakan (Movement)
MovementTab:AddSlider("WalkSpeed Hack", {
    Min = 16,
    Max = 200,
    Default = 16,
    Unit = " studs/s",
    Toggle = {
        Default = false,
        Callback = function(enabled)
            print("Speedhack state:", enabled)
        end
    },
    Keybind = Enum.KeyCode.F,
    Callback = function(speed)
        print("WalkSpeed target:", speed)
    end
})

MovementTab:AddButton("Reset Karakter", {
    Callback = function()
        game.Players.LocalPlayer.Character:BreakJoints()
    end
})

-- 5. Inisialisasi Elemen di Tab ESP (Visuals)
ESPTab:AddDropdown("ESP Aktif", {
    Options = {"Enabled", "Disabled"},
    Default = "Disabled",
    Callback = function(choice)
        print("ESP Status:", choice)
    end
})

ESPTab:AddColorpicker("Warna Kotak ESP", {
    Default = Color3.fromRGB(0, 120, 255),
    Callback = function(color)
        print("ESP Color RGB:", tostring(color))
    end
})

-- 6. Hook Simpan/Muat Profil
Window:BindConfigActions({
    OnLoad = function()
        SysUI:Notify({ Title = "Config", Content = "Profil konfigurasi berhasil dimuat!", Duration = 2 })
    end,
    OnSave = function()
        SysUI:Notify({ Title = "Config", Content = "Profil konfigurasi disimpan!", Duration = 2 })
    end
})

-- 7. Kirim Notifikasi Sukses Memuat
SysUI:Notify({
    Title = "SysUI Loader",
    Content = "Sistem berhasil dijalankan. Klik RightShift untuk sembunyikan menu.",
    Duration = 4
})
```
