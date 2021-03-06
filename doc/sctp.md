# sctp


## BPF

```
000: A = P[12:2]
001: if (A == 34525) goto 2 else goto 7
002: A = P[20:1]
003: if (A == 132) goto 10 else goto 4
004: if (A == 44) goto 5 else goto 11
005: A = P[54:1]
006: if (A == 132) goto 10 else goto 11
007: if (A == 2048) goto 8 else goto 11
008: A = P[23:1]
009: if (A == 132) goto 10 else goto 11
010: return 65535
011: return 0
```


## BPF cross-compiled to Lua

```
return function (P, length)
   local A = 0
   if 14 > length then return false end
   A = bit.bor(bit.lshift(P[12], 8), P[12+1])
   if not (A==34525) then goto L6 end
   if 21 > length then return false end
   A = P[20]
   if (A==132) then goto L9 end
   if not (A==44) then goto L10 end
   if 55 > length then return false end
   A = P[54]
   if (A==132) then goto L9 end
   goto L10
   ::L6::
   if not (A==2048) then goto L10 end
   if 24 > length then return false end
   A = P[23]
   if not (A==132) then goto L10 end
   ::L9::
   do return true end
   ::L10::
   do return false end
   error("end of bpf")
end
```


## Direct pflang compilation

```
local cast = require("ffi").cast
return function(P,length)
   if length < 34 then return false end
   local var1 = cast("uint16_t*", P+12)[0]
   if var1 == 8 then
      return P[23] == 132
   end
   if length < 54 then return false end
   if var1 ~= 56710 then return false end
   local var4 = P[20]
   if var4 == 132 then return true end
   if length < 55 then return false end
   if var4 ~= 44 then return false end
   return P[54] == 132
end

```

