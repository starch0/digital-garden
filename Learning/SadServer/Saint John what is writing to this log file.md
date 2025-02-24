Issue:  A developer created a testing program that is continuously writing to a log file `_/var/log/bad.log` and filling up disk. You can check for example with `tail -f /var/log/bad.log`  
This program is no longer needed. Find it and terminate it.

My solution:
I checked the procces using `tail -f /var/log/bad.log`, then `top` to see if i could catch it(i couldnt), then `lsof /var/log/bad.log`  and `kill [PID]` 
