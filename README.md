# UESIM
```
sudo gdb --arg sudo NFAPI_TRACE_LEVEL=debug ./nr-uesoftmodem -r 106 --numerology 1 --band 78 -C 3619200000 --sa --uicc0.imsi 001010000000001 --rfsim
```

# inside gdb 

- set obj file to track
```
file ./nr-uesoftmodem
```

- run obj file
```
run
```

- After reaching Breaking Point 
```
bt

print <variable>

info local
```
