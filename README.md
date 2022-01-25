# Marlin 3D Printer Firmware Custom for Milling

## Hardware
- Mega2560
- BOARD_RAMPS_14_SF
- enstop

## Custom firmware

### Chọn lại motherboard phù hợp với board đã mua.

```
#ifndef MOTHERBOARD
  #define MOTHERBOARD BOARD_RAMPS_14_SF
#endif
```

### Thay đổi tốc độ giao tiếp uart.

```
#define BAUDRATE 115200
```

### Thay đổi tên của máy cnc.

```
#define CUSTOM_MACHINE_NAME "WEGSTR"
```

### Endstop Settings (Cài đặt công tắc hành trình).

- khai báo dùng các vị trí công tắc hành trình.

```
#define USE_XMIN_PLUG
#define USE_YMIN_PLUG
#define USE_ZMIN_PLUG
//#define USE_IMIN_PLUG
//#define USE_JMIN_PLUG
//#define USE_KMIN_PLUG
//#define USE_XMAX_PLUG
//#define USE_YMAX_PLUG
// #define USE_ZMAX_PLUG
//#define USE_IMAX_PLUG
//#define USE_JMAX_PLUG
//#define USE_KMAX_PLUG
```
- `USE_ZMIN_PLUG` Được dùng làm auto bed leveling nên 1 đầu được gắn với đầu khoan cnc đầu còn lại gắn vào bàn.

- Khai báo chuyển logic cho công tắc hành trình

```
#define X_MIN_ENDSTOP_INVERTING true // Set to true to invert the logic of the endstop.
#define Y_MIN_ENDSTOP_INVERTING true // Set to true to invert the logic of the endstop.
#define Z_MIN_ENDSTOP_INVERTING true // Set to true to invert the logic of the endstop.
#define I_MIN_ENDSTOP_INVERTING false 
#define J_MIN_ENDSTOP_INVERTING false
#define K_MIN_ENDSTOP_INVERTING false 
#define X_MAX_ENDSTOP_INVERTING false 
#define Y_MAX_ENDSTOP_INVERTING false 
#define Z_MAX_ENDSTOP_INVERTING false 
#define I_MAX_ENDSTOP_INVERTING false 
#define J_MAX_ENDSTOP_INVERTING false 
#define K_MAX_ENDSTOP_INVERTING false 
#define Z_MIN_PROBE_ENDSTOP_INVERTING true 

```

### Khai báo loại driver điều khiển động cơ

```
#define X_DRIVER_TYPE  A4988
#define Y_DRIVER_TYPE  A4988
#define Z_DRIVER_TYPE  A4988
```

### Cài đặt số bước trên 1mm

1. Công thức tính đối với loại gắn trục không có dây đai.
`B = 360/(α*λ*m)`
trong đó:
  - `m = {1, 1/2, 1/4, 1/8, 1/16, 1/32...)` A4988 thì `m = 1/16` (GHIM 3 JUM TRONG ), DRV8825 thì `m= 1/32`
  - `λ` (mm) là bước ren thường thì `λ = {2, 4, 8...} mm`
  - `α` là số độ trên 1 bước. động cơ có 200 bước mỗi bước 1.8 => `α = 1.8°`

- Động cơ bước có α = 1.8°; trục vitme bước ren 8mm; và dùng module điều khiển là A4988 thì ta tính được bước là.

=>` B = 360/{1.8*8*(1/16)} = 400 (steps/mm)`
```
#define DEFAULT_AXIS_STEPS_PER_UNIT   { 400, 400, 1600, 92.6 }
```

- [xem thêm ở đây](https://www.beelab.info/2017/06/3d-printer-ky-thuat-hieu-chinh-cai-at.html?m=0) và [ở đây](https://www.diyeverything.xyz/2020/06/dong-co-buoc-va-cach-tinh-buoc-stepper-motor-nema.html)

2. Công thức tính đối với loại có dây đai.

### Thay đổi tốc độ di chuyển trên các trục

- Đối với phay mạch nên để là 1 cho `x` và `y`
```
#define DEFAULT_MAX_FEEDRATE          { 1, 1, 5, 25 }
```

### Sử dụng trục min z cho enstop

```
#define Z_MIN_PROBE_USES_Z_MIN_ENDSTOP_PIN

#define NOZZLE_AS_PROBE
```

### Bù trừ vị trí trục z (trường hợp dùng btouch hoặc tương tự).

- Hiện tại dùng trục z là điểm dò nên để bằng 0

```
#define NOZZLE_TO_PROBE_OFFSET { 0, 0, 0 }
```

### Cài đặt khung làm việc cách điểm zero

- Cài đặt vùng làm việc cách điểm zero là 1mm
```
#define PROBING_MARGIN 1
```

### Kích thước bàn cnc

```
#define X_BED_SIZE 65
#define Y_BED_SIZE 45

// Travel limits (mm) after homing, corresponding to endstop positions.
#define X_MIN_POS 0
#define Y_MIN_POS 0
#define Z_MIN_POS 0
#define X_MAX_POS X_BED_SIZE
#define Y_MAX_POS Y_BED_SIZE
#define Z_MAX_POS 50 // 110
```

### Cài đặt auto bed leveling

```
// #define AUTO_BED_LEVELING_3POINT
// #define AUTO_BED_LEVELING_LINEAR
#define AUTO_BED_LEVELING_BILINEAR
// #define AUTO_BED_LEVELING_UBL
// #define MESH_BED_LEVELING

```

```
#define RESTORE_LEVELING_AFTER_G28
// #define ENABLE_LEVELING_AFTER_G28

```

```
#define LCD_BED_LEVELING
```

```
#define Z_SAFE_HOMING
```

- Cài đặt số điểm được đo trên mặt bàn khi auto leveling 
- trục `x=15` và `y=15`

```
  #define GRID_MAX_POINTS_X 15
  #define GRID_MAX_POINTS_Y GRID_MAX_POINTS_X

```

### Cài đặt epprom

```
#define EEPROM_SETTINGS 
```

### Cài đặt LCD

### Cài đặt SD

```
#define SDSUPPORT
#define SD_CHECK_AND_RETRY
```

### Cài đặt speaker

```
#define SPEAKER
```

### Thiết lập loại LCD

```
#define REPRAP_DISCOUNT_FULL_GRAPHIC_SMART_CONTROLLER
```


# Đối với loại mũi phay cnc
- Nên dùng loại mũi trái dứa hoặc loại phay mạch nên chọn từ 0.6 đến 1mm
