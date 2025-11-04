# Audibl
Turns Linux box into suspended (deep sleep S3) mode on finishing playing audio (as well as video with audio track).

```
#!/bin/bash

echo "Audio playback status. Press CTRL+C to stop."

declare -i limit=0; 
while sleep 5; do 
  cnt=$(grep RUNNING \
  /proc/asound/card*/pcm*/sub*/status | wc -l)
  echo -n "${cnt}";
  if [ $cnt -eq 0 ]; then
    if [ $limit -lt 9 ]; then #some chill out before suspending
      limit=$((limit+1))
    else
      limit=0
      systemctl suspend
      exit
    fi
  else
    limit=0
  fi
done
```
