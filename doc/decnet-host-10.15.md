# decnet host 10.15


## BPF

```
000: A = P[12:2]
001: if (A == 24579) goto 2 else goto 43
002: A = P[16:1]
003: A &= 7
004: if (A == 2) goto 5 else goto 7
005: A = P[19:2]
006: if (A == 3880) goto 42 else goto 7
007: A = P[16:2]
008: A &= 65287
009: if (A == 33026) goto 10 else goto 12
010: A = P[20:2]
011: if (A == 3880) goto 42 else goto 12
012: A = P[16:1]
013: A &= 7
014: if (A == 6) goto 15 else goto 17
015: A = P[31:2]
016: if (A == 3880) goto 42 else goto 17
017: A = P[16:2]
018: A &= 65287
019: if (A == 33030) goto 20 else goto 22
020: A = P[32:2]
021: if (A == 3880) goto 42 else goto 22
022: A = P[16:1]
023: A &= 7
024: if (A == 2) goto 25 else goto 27
025: A = P[17:2]
026: if (A == 3880) goto 42 else goto 27
027: A = P[16:2]
028: A &= 65287
029: if (A == 33026) goto 30 else goto 32
030: A = P[18:2]
031: if (A == 3880) goto 42 else goto 32
032: A = P[16:1]
033: A &= 7
034: if (A == 6) goto 35 else goto 37
035: A = P[23:2]
036: if (A == 3880) goto 42 else goto 37
037: A = P[16:2]
038: A &= 65287
039: if (A == 33030) goto 40 else goto 43
040: A = P[24:2]
041: if (A == 3880) goto 42 else goto 43
042: return 65535
043: return 0
```


## BPF cross-compiled to Lua

```
return function (P, length)
   local A = 0
   if 14 > length then return 0 end
   A = bit.bor(bit.lshift(P[12], 8), P[12+1])
   if not (A==24579) then goto L42 end
   if 17 > length then return 0 end
   A = P[16]
   A = bit.band(A, 7)
   if not (A==2) then goto L6 end
   if 21 > length then return 0 end
   A = bit.bor(bit.lshift(P[19], 8), P[19+1])
   if (A==3880) then goto L41 end
   ::L6::
   if 18 > length then return 0 end
   A = bit.bor(bit.lshift(P[16], 8), P[16+1])
   A = bit.band(A, 65287)
   if not (A==33026) then goto L11 end
   if 22 > length then return 0 end
   A = bit.bor(bit.lshift(P[20], 8), P[20+1])
   if (A==3880) then goto L41 end
   ::L11::
   if 17 > length then return 0 end
   A = P[16]
   A = bit.band(A, 7)
   if not (A==6) then goto L16 end
   if 33 > length then return 0 end
   A = bit.bor(bit.lshift(P[31], 8), P[31+1])
   if (A==3880) then goto L41 end
   ::L16::
   if 18 > length then return 0 end
   A = bit.bor(bit.lshift(P[16], 8), P[16+1])
   A = bit.band(A, 65287)
   if not (A==33030) then goto L21 end
   if 34 > length then return 0 end
   A = bit.bor(bit.lshift(P[32], 8), P[32+1])
   if (A==3880) then goto L41 end
   ::L21::
   if 17 > length then return 0 end
   A = P[16]
   A = bit.band(A, 7)
   if not (A==2) then goto L26 end
   if 19 > length then return 0 end
   A = bit.bor(bit.lshift(P[17], 8), P[17+1])
   if (A==3880) then goto L41 end
   ::L26::
   if 18 > length then return 0 end
   A = bit.bor(bit.lshift(P[16], 8), P[16+1])
   A = bit.band(A, 65287)
   if not (A==33026) then goto L31 end
   if 20 > length then return 0 end
   A = bit.bor(bit.lshift(P[18], 8), P[18+1])
   if (A==3880) then goto L41 end
   ::L31::
   if 17 > length then return 0 end
   A = P[16]
   A = bit.band(A, 7)
   if not (A==6) then goto L36 end
   if 25 > length then return 0 end
   A = bit.bor(bit.lshift(P[23], 8), P[23+1])
   if (A==3880) then goto L41 end
   ::L36::
   if 18 > length then return 0 end
   A = bit.bor(bit.lshift(P[16], 8), P[16+1])
   A = bit.band(A, 65287)
   if not (A==33030) then goto L42 end
   if 26 > length then return 0 end
   A = bit.bor(bit.lshift(P[24], 8), P[24+1])
   if not (A==3880) then goto L42 end
   ::L41::
   do return 65535 end
   ::L42::
   do return 0 end
   error("end of bpf")
end
```


## Direct pflang compilation

```
return function(P,length)
   if not (length >= 21) then do return false end end
   do
      local v1 = P[16]
      local v2 = bit.band(v1,7)
      if not (v2 == 2) then goto L3 end
      do
         local v3 = ffi.cast("uint16_t*", P+19)[0]
         if v3 == 3850 then do return true end end
         do
            local v4 = ffi.cast("uint16_t*", P+17)[0]
            do return v4 == 3850 end
         end
      end
::L3::
      do
         if not (length >= 22) then do return false end end
         do
            local v5 = ffi.cast("uint16_t*", P+16)[0]
            local v6 = bit.band(v5,2047)
            if not (v6 == 641) then goto L7 end
            do
               local v7 = ffi.cast("uint16_t*", P+20)[0]
               if v7 == 3850 then do return true end end
               do
                  local v8 = ffi.cast("uint16_t*", P+18)[0]
                  do return v8 == 3850 end
               end
            end
::L7::
            do
               if not (length >= 33) then do return false end end
               do
                  if not (v2 == 6) then goto L11 end
                  do
                     local v9 = ffi.cast("uint16_t*", P+31)[0]
                     if v9 == 3850 then do return true end end
                     do
                        local v10 = ffi.cast("uint16_t*", P+23)[0]
                        do return v10 == 3850 end
                     end
                  end
::L11::
                  do
                     if not (length >= 34) then do return false end end
                     do
                        if not (v6 == 1665) then do return false end end
                        do
                           local v11 = ffi.cast("uint16_t*", P+32)[0]
                           if v11 == 3850 then do return true end end
                           do
                              local v12 = ffi.cast("uint16_t*", P+24)[0]
                              do return v12 == 3850 end
                           end
                        end
                     end
                  end
               end
            end
         end
      end
   end
end
```

