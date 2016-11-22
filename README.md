# multiwii-firmware
## This repository contains some new features and improvemrnts:
- Failsafe + RTH + Land
- RTH with automatic landing
- Fast land mode

## New in Config.h
- FAILSAFE_RTH_AND_LAND - enable RTH feature with automatic landing (independent on RTH_AND_LAND state) at home on failsafe (enabled if uncommented)
- RTH_AND_LAND - enable automatic landing after RTH. Affects RTH mode, which was enabled by pilot, doesn't affect RTH on failsafe. (0 - disabled; 1 - enabled)
- FAST_LAND - replaces default landing by fast landing (0 - disabled; 1 - enabled)
- FAST_LAND_LEVELS - landing levels array in Cm. If vehicle is above last level, it descends with max possible speed. If vehicle is above some level, it lands with speed taken from FAST_LAND_SPEEDS
- FAST_LAND_SPEEDS - landing speeds array for levels from FAST_LAND_SPEEDS. 100 speed units = approx 50cm/sec

## FAST_LAND notes
When FAST_LAND is enabled, default LAND_SPEED parameter is not used.

### Setting example 1:
```
#define FAST_LAND 1
#define FAST_LAND_LEVELS { 500, 1000, 2000, 5000, 10000 }
#define FAST_LAND_SPEEDS { 100, 200,  500,  1000,  1500 }
```
Height > 100m ---> Max possible speed (depends on Alt Hold PIDs)<br />
Height 50-100m ---> Speed = 7.5m/s<br />
Height 20-50m ---> Speed = 5m/s<br />
Height 10-20m ---> Speed = 2.5m/s<br />
Height 5-10m ---> Speed = 1m/s<br />
Height < 5m ---> Speed = 0.5m/s

### Setting example 1:
```
#define FAST_LAND 1
#define FAST_LAND_LEVELS { 1500 }
#define FAST_LAND_SPEEDS { 200 }
```
Vehicle lands to 15m with max speed, then with 1m/s.

## FAST_LAND WARNINGS
- Fast land mode uses Altitude Hold and GPS position hold modes, so ensure, that those modes work fine (PIDs are configured) before using Fast Landing.
- Do not make highest level too low (lower than 10-15m), because vehicle may not have enough time to break, epecially if Alt Hold PIDs are not configured correctly.
