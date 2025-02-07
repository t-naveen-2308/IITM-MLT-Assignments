[TOC]



## Week-2 Problem-1

Write a Python function ***combinationSort(strList)*** that takes a list of unique strings `strList` as an argument, where each string is a combination of a letter from `a` to `z` and a number from `0` to `99`, the initial character in string being the letter. For example `a23`, `d5`, `q99` are some strings in this format. This function should sorts the list  and return two lists `(L1, L2)` in the order mentioned below. 

`L1`: First list should contain all strings sorted in ascending order with respect to the first character only. All strings with same initial character should be in the same order as in the original list. 

`L2`: In the list `L1` above, sort the strings starting with same character, in descending order with respect to the number formed by the remaining characters.  

**Example:**

***Sample input 1:*** 

```
d34, g54, d12, b87, g1, c65, g40, g5, d77
```

***Sample output 1:***

```
L1: b87, c65, d34, d12, d77, g54, g1, g40, g5
L2: b87, c65, d77, d34, d12, g54, g40, g5 ,g1
```

### Solution

**Solution 1:** Using Insertion sort.

```python
def combinationSort(strList):
  # Using insertion sort to sort on initial character, as this is a stable sort.
  # Other stable sorting techniques can be used.
  for i in range(1, len(strList)):
    j=i
    while (j>0 and strList[j][0] < strList[j-1][0]):
      strList[j], strList[j-1] = strList[j-1], strList[j]
      j -= 1
  print("L1:"+str(strList))

  L1 = strList.copy() # Saving intermediate result to return later.
  right = 1
  left = 0

  # As there can be no more than 100 strings with same initial character.
  # Using insertion sort to sort these.
  while i<len(strList):
    right = i
    while(right>left and strList[right][0] == strList[right-1][0] and int(strList[right-1][1:])<int(strList[right][1:])):
      strList[right], strList[right-1] = strList[right-1], strList[right]
      right -= 1
    i += 1
  
  return L1, strList
```

**Solution 2:** Using dictionary to group strings with same initial character. Than using insertion sort to sort the elements in the group.

```python
import string

def combinationSort(strList):
  # Create a dictionary with 26 keys from characters 'a' to 'z', each key has an empty list as value.
  groups = {k: [] for k in string.ascii_lowercase}

  # Using this dictionary to group strings with same initial character.  
  for i in range(len(strList)):
    char=strList[i][0]
    groups[char].append(strList[i])
  
  strList=[]
  # Recreate the list from all the strings in groups, iterating on groups from a to z.
  for char in groups.keys():
    for s in groups[char]:
      strList.append(s)
  
  L1 = strList.copy() # Saving intermediate result to return later.
  i = 1
  left = 0
  # As there can be no more than 100 strings with same initial character.
  # Using insertion sort within group.
  while i<len(strList):
    right = i
    while(right>left and strList[right][0] == strList[right-1][0] and int(strList[right-1][1:])<int(strList[right][1:])):
      strList[right], strList[right-1] = strList[right-1], strList[right]
      right -= 1
    i += 1
  
  return L1, strList
```



**Suffix(Hidden)**

```python
L = input().split(", ")
L1, L2 = combinationSort(L)
print("L1: ", end="")
print(*L1, sep=", ")
print("L2: ", end="")
print(*L2, sep=", ")
```

**Public test case 1**

**Input**

```
d34, g54, d12, b87, g1, c65, g40, g5, d77
```

**output**

```
L1: b87, c65, d34, d12, d77, g54, g1, g40, g5
L2: b87, c65, d77, d34, d12, g54, g40, g5 ,g1
```

**Private test case 1**

**Input**

```
e28, i25, q34, m76, o23, r25, x80, x38, s89, u94, e93, m6, j69, f31, z28, o39, h11, q68, t82, t77, y84, d2, h56, u71, k11, x89, w26, d54, a19, l99, v67, f91, l55, e48, c40, l63, s41, s49, v82, i47, w95, i63, f22, c53, s24, s52, z60, n92, m98, l88, e62, z1, u32, c98, k94, k37, n42, v50, j73, w90, a78, f96, e47, c29, y87, d84, b59, l32, a56, p50, v70, r54, z95, c68, z32, d23, n18, h65, m39, k56, h64, t45, i58, d94, u76, s77, v34, k45, m22, e72, f95, y55, c9, x87, a98, l20, u29, w81, d35, z31
```

**Output**

```
L1: a19, a78, a56, a98, b59, c40, c53, c98, c29, c68, c9, d2, d54, d84, d23, d94, d35, e28, e93, e48, e62, e47, e72, f31, f91, f22, f96, f95, h11, h56, h65, h64, i25, i47, i63, i58, j69, j73, k11, k94, k37, k56, k45, l99, l55, l63, l88, l32, l20, m76, m6, m98, m39, m22, n92, n42, n18, o23, o39, p50, q34, q68, r25, r54, s89, s41, s49, s24, s52, s77, t82, t77, t45, u94, u71, u32, u76, u29, v67, v82, v50, v70, v34, w26, w95, w90, w81, x80, x38, x89, x87, y84, y87, y55, z28, z60, z1, z95, z32, z31
L2: a98, a78, a56, a19, b59, c98, c68, c53, c40, c29, c9, d94, d84, d54, d35, d23, d2, e93, e72, e62, e48, e47, e28, f96, f95, f91, f31, f22, h65, h64, h56, h11, i63, i58, i47, i25, j73, j69, k94, k56, k45, k37, k11, l99, l88, l63, l55, l32, l20, m98, m76, m39, m22, m6, n92, n42, n18, o39, o23, p50, q68, q34, r54, r25, s89, s77, s52, s49, s41, s24, t82, t77, t45, u94, u76, u71, u32, u29, v82, v70, v67, v50, v34, w95, w90, w81, w26, x89, x87, x80, x38, y87, y84, y55, z95, z60, z32, z31, z28, z1
```

**Private test case 2**

**Input**

```
a23, a26, a35, a52, a58, a65, a72, a80, a83, a96, b0, b15, b41, b50, b55, b57, b75, b91, c42, c50, c56, c88, c89, c95, c99, d1, d12, d13, d25, d33, d4, d57, d58, d61, d90, d91, d92, e20, e45, e57, e60, e76, e92, f14, f33, f37, f58, f64, f83, f91, f97, g23, g36, g69, g73, g75, g99, h13, h15, h17, h28, h34, h63, h89, h90, h97, i19, i22, i28, i33, i38, i43, i45, i47, i72, i88, j29, j40, j53, j62, j66, j67, j88, j89, j92, j97, k17, k19, k22, k39, k44, k56, k58, k64, l46, l54, l58, l85, l90, l92, l97, l98, m14, m16, m21, m3, m42, m44, m66, m85, m87, m97, n17, n23, n4, n42, n64, n66, n83, n94, o34, o43, o50, o65, o84, o86, o95, p0, p17, p2, p34, p35, p43, p8, p95, q26, q54, q62, q72, r11, r22, r36, r54, r77, r79, r80, r90, r95, s1, s70, s75, s8, s86, s9, s91, s93, s98, t19, t38, t43, t77, t86, u1, u16, u69, u91, v22, v46, v61, v66, v70, v96, v99, w13, w36, w38, w46, w54, w56, w92, x22, x25, x39, x40, x47, x72, x8, x83, x87, y71, y72, y86, y99, z12, z27, z43, z53, z60, z78, z92
```

**Output**

```
L1: a23, a26, a35, a52, a58, a65, a72, a80, a83, a96, b0, b15, b41, b50, b55, b57, b75, b91, c42, c50, c56, c88, c89, c95, c99, d1, d12, d13, d25, d33, d4, d57, d58, d61, d90, d91, d92, e20, e45, e57, e60, e76, e92, f14, f33, f37, f58, f64, f83, f91, f97, g23, g36, g69, g73, g75, g99, h13, h15, h17, h28, h34, h63, h89, h90, h97, i19, i22, i28, i33, i38, i43, i45, i47, i72, i88, j29, j40, j53, j62, j66, j67, j88, j89, j92, j97, k17, k19, k22, k39, k44, k56, k58, k64, l46, l54, l58, l85, l90, l92, l97, l98, m14, m16, m21, m3, m42, m44, m66, m85, m87, m97, n17, n23, n4, n42, n64, n66, n83, n94, o34, o43, o50, o65, o84, o86, o95, p0, p17, p2, p34, p35, p43, p8, p95, q26, q54, q62, q72, r11, r22, r36, r54, r77, r79, r80, r90, r95, s1, s70, s75, s8, s86, s9, s91, s93, s98, t19, t38, t43, t77, t86, u1, u16, u69, u91, v22, v46, v61, v66, v70, v96, v99, w13, w36, w38, w46, w54, w56, w92, x22, x25, x39, x40, x47, x72, x8, x83, x87, y71, y72, y86, y99, z12, z27, z43, z53, z60, z78, z92
L2: a96, a83, a80, a72, a65, a58, a52, a35, a26, a23, b91, b75, b57, b55, b50, b41, b15, b0, c99, c95, c89, c88, c56, c50, c42, d92, d91, d90, d61, d58, d57, d33, d25, d13, d12, d4, d1, e92, e76, e60, e57, e45, e20, f97, f91, f83, f64, f58, f37, f33, f14, g99, g75, g73, g69, g36, g23, h97, h90, h89, h63, h34, h28, h17, h15, h13, i88, i72, i47, i45, i43, i38, i33, i28, i22, i19, j97, j92, j89, j88, j67, j66, j62, j53, j40, j29, k64, k58, k56, k44, k39, k22, k19, k17, l98, l97, l92, l90, l85, l58, l54, l46, m97, m87, m85, m66, m44, m42, m21, m16, m14, m3, n94, n83, n66, n64, n42, n23, n17, n4, o95, o86, o84, o65, o50, o43, o34, p95, p43, p35, p34, p17, p8, p2, p0, q72, q62, q54, q26, r95, r90, r80, r79, r77, r54, r36, r22, r11, s98, s93, s91, s86, s75, s70, s9, s8, s1, t86, t77, t43, t38, t19, u91, u69, u16, u1, v99, v96, v70, v66, v61, v46, v22, w92, w56, w54, w46, w38, w36, w13, x87, x83, x72, x47, x40, x39, x25, x22, x8, y99, y86, y72, y71, z92, z78, z60, z53, z43, z27, z12
```

**Private test case 3**

**Input**

```
k45, g9, y11, y65, y50, t77, b14, v35, n51, l0, z69, s66, x52, s97, d67, d3, j76, e33, a84, v81, c96, m37, i87, l7, u0, y99, p49, s47, l20, x59, l92, u88, l58, f57, z56, t7, d9, f40, z96, p32, q27, o67, l12, c3, b56, o35, m30, c94, h85, g39, w64, t48, w20, a80, i71, j77, q31, i96, r47, i83, o29, k43, g97, d81, e37, h53, n99, e31, l80, k55, d41, y69, b18, p43, x53, r67, a77, y1, j22, d24, z16, y79, u79, n7, c20, c62, h33, c29, k36, f25, f66, z37, x62, h91, m56, s69, u51, q88, b68, f96, j84, e82, s95, q93, u87, z1, t61, c13, j5, y8, r86, r66, n86, q87, x67, e73, k19, q13, o64, h78, o66, p10, s26, c25, p79, e65, g53, z43, g96, u48, i23, q85, y61, i97, m65, m25, n30, u66, m48, n74, x21, q22, l94, t22, u71, l46, o0, v93, v67, z97, b84, e80, l48, c81, k87, m46, r29, d91, b17, q83, k79, k33, t17, e28, y31, i29, d18, c70, s24, s91, o13, p3, z78, e56, r23, z29, b87, e99, b77, q30, n59, l73, m31, d87, l25, e54, v54, h18, n73, b11, v1, w67, s7, r55, n96, u64, c89, l6, g16, a24, k71, p8, k23, a1, p24, n11, c34, b39, l62, l21, p74, g38, m50, t44, h89, k61, d55, v15, v90, c85, j47, c48, j80, q66, y23, t35, q79, a64, f61, x49, e62, r39, p33, r46, y38, t32, w51, h44, v82, b86, f73, i68, f33, g23, j0, j32, a28, i35, s98, u27, b27, h15, i34, l52, t81, x33, t64, u3, n44, p65, q44, e32, b41, k99, o87, f91, o47, p86, h61, n68, f68, x87, t71, u60, v53, s48, s33, v42, s31, e94, g84, j25, x44, y90, w30, b30, k54, s93, a45, w24, b21, f87, h72, a51, d96, v98, p71, h50, z65, t10, v22, n1, y48, i59, u58, p75, v14, k41, e39, j43, q36, j29, q94, i16, u7, w92, y46, i10, j21, d90, q71, d32, b94, o90, f76, h32, c15, b40, j2, v77, r71, w25, h27, p91, a49, p17, q68, s19, y80, g57, f47, h93, i81, m70, s53, c71, g99, f24, m71, a62, h73, g12, r22, j27, t72, n98, c88, r53, v27, j83, t58, a88, u94, m87, z38, a42, b83, k35, x1, x70, g15, z41, h56, t25, r88, e91, z21, o55, l13, d58, w90, i70, d22, k7, c38, n48, a36, a66, m3, i2, d40, u12, d88, a86, j19, o63, l19, t3, d42, l99, f84, u97, t70, f16, g90, l57, e84, y97, y53, p52, z53, t56, t24, k26, e18, i99, o69, j81, z80, h75, m81, w4, l98, n32, a31, d43, w98, j11, v16, f3, x11, a10, s37, c6, l53, i33, f67, l67, l41, h7, q28, t66, s55, v43, y17, j69, e26, x74, r63, v26, v92, z6, v74, u54, e76, v8, d54, z45, e29, u57, l2, j23, e16, l27, j92, d12, f27, c19, j15, v59, l4, z5, t30, t16, h40, x96, c35, j31, t2, v0, w7, m52, k18, l47, l61, g50, n36, g30, x77, z82, p13, z48, p36, x4, p7, i50, y75, s96, w60, n22, n66, p6, m18, g89, h12, j49, d95, x0, r38, x51, h5, t53, o37, u61, g48, u21, t6, o2, g28, t91, f72, x16, h90, t23, j38, e92, l71, a33, p39, h79, g64, n88, m63, d27, b82, c60, q58, j54, h4, y26, l1, t39, x28, s15, w6, w88, j40, u6, x27, b97, u89, k5, l85, b47, g11, g34, g25, y9, d56, t49, p63, z75, e46, p4, m42, k13, n53, z62, s6, o22, a70, d53, h64, w58, r7, e87, r62, p85, r83, o54, y4, g91, w27, g65, u30, k72, o15, w87, s8, p57, i69, r85, r80, k73, p88, o16, n93, i54, r9, q12, g79, r81, t74, x85, x75, w28, g52, h74, v71, d2, p31, u32, l24, d60, d84, l29, y42, h0, a46, w57, b43, k49, p59, v7, t67, m6, o96, h55, b76, w72, m1, h47, i94, m39, o75, k30, b8, n4, t19, m62, a76, g85, d25, l16, y89, s83, z31, k67, r94, i28, c90, m15, z99, s58, p15, x58, w82, k78, i86, p50, q2, i49, h67, j65, y30, n13, u8, f75, k10, r15, d4, j99, v72, s25, e19, s38, u75, p28, m29, j33, m78, r68, w78, j72, b22, q18, g47, w70, e25, l56, j62, b95, a56, c41, m24, d99, e67, o51, r8, j82, u10, o89, n21, a20, w66, e41, k93, b9, v85, s5, o6, k11, t45, d73, v5, w79, m10, q82, i53, y47, g0, p82, p9, j42, u46, a61, k12, t94, u84, z27, e5, r41, o73, z15, b69, u77, h29, h11, e4, w11, s51, n69, d59, r13, i92, f64, j16, u53, i6, h82, k46, a63, x99, f65, k56, r87, s68, u95, g40, a38, e36, x57, b33, f82, f7, i47, m54, n97, j79, j70, c61, g70, y43, a67, a69, y88, p87, d7, x93, o95, o79, r0, m88, b91, v51, t69, e24, l82, c42, h99, o80, n26, v79, j45, i37, w50, h51, f31, j73, z63, c78, a73, b71, c36, y83, a57, w76, i13, v21, d6, o14, w59, e64, x17, g19, m55, j71, z85, d35, p64, g82, b54, m23, b2, q34, a30, m94, w42, y25, t90, i14, z35, r76, v33, t0, r30, m21, y15, w13, y86, q52, q55, y68, d66, e77, u81, o58, o92, h76, l81, v28, p58, a98, g31, d29, a27, j78, r4, r43, r16, u73, z79, y2, w44, h24, s22, d11, l86, i72, k21, s14, i67, o48, d19, f56, e2, y49, u42, t20, w94, i63, z71, b1, m16, s17, x29, w84, q41, g43, w96, r95, z98, y92, u93, r50, d80, q5, j94, k62, p95, l35, l87, l50, o59, g61, j55, e38, q20, p83, q43, u98, v44, e42, q92, x9, b61, h69, x26, m12, q53, c5, n94, t78, f94, w22, l76, w91, g95, i85, y76, k84, b89, k58, v99, r92, b32, f28, a50, j41, r28, h26, j58, a44, c93, k32, h36, h87, n82, w32, y35, z4, z28, m85, n91, c99, z54, z0, f12, j9, b36, x71, s40, c54, n61, w17, p46, g62, t88, y14, r37, m41, r90, u59, z86, c69, f37, n83, d28, m53, b35, n71, k51, d21, n2, v88, h95, h6, y63, c47, u99, v11, g1, v2, v45, w36, m11, x35, x36, e30, b60
```

**Output**

```
L1: a84, a80, a77, a24, a1, a64, a28, a45, a51, a49, a62, a88, a42, a36, a66, a86, a31, a10, a33, a70, a46, a76, a56, a20, a61, a63, a38, a67, a69, a73, a57, a30, a98, a27, a50, a44, b14, b56, b18, b68, b84, b17, b87, b77, b11, b39, b86, b27, b41, b30, b21, b94, b40, b83, b82, b97, b47, b43, b76, b8, b22, b95, b9, b69, b33, b91, b71, b54, b2, b1, b61, b89, b32, b36, b35, b60, c96, c3, c94, c20, c62, c29, c13, c25, c81, c70, c89, c34, c85, c48, c15, c71, c88, c38, c6, c19, c35, c60, c90, c41, c61, c42, c78, c36, c5, c93, c99, c54, c69, c47, d67, d3, d9, d81, d41, d24, d91, d18, d87, d55, d96, d90, d32, d58, d22, d40, d88, d42, d43, d54, d12, d95, d27, d56, d53, d2, d60, d84, d25, d4, d99, d73, d59, d7, d6, d35, d66, d29, d11, d19, d80, d28, d21, e33, e37, e31, e82, e73, e65, e80, e28, e56, e99, e54, e62, e32, e94, e39, e91, e84, e18, e26, e76, e29, e16, e92, e46, e87, e19, e25, e67, e41, e5, e4, e36, e24, e64, e77, e2, e38, e42, e30, f57, f40, f25, f66, f96, f61, f73, f33, f91, f68, f87, f76, f47, f24, f84, f16, f3, f67, f27, f72, f75, f64, f65, f82, f7, f31, f56, f94, f28, f12, f37, g9, g39, g97, g53, g96, g16, g38, g23, g84, g57, g99, g12, g15, g90, g50, g30, g89, g48, g28, g64, g11, g34, g25, g91, g65, g79, g52, g85, g47, g0, g40, g70, g19, g82, g31, g43, g61, g95, g62, g1, h85, h53, h33, h91, h78, h18, h89, h44, h15, h61, h72, h50, h32, h27, h93, h73, h56, h75, h7, h40, h12, h5, h90, h79, h4, h64, h74, h0, h55, h47, h67, h29, h11, h82, h99, h51, h76, h24, h69, h26, h36, h87, h95, h6, i87, i71, i96, i83, i23, i97, i29, i68, i35, i34, i59, i16, i10, i81, i70, i2, i99, i33, i50, i69, i54, i94, i28, i86, i49, i53, i92, i6, i47, i37, i13, i14, i72, i67, i63, i85, j76, j77, j22, j84, j5, j47, j80, j0, j32, j25, j43, j29, j21, j2, j27, j83, j19, j81, j11, j69, j23, j92, j15, j31, j49, j38, j54, j40, j65, j99, j33, j72, j62, j82, j42, j16, j79, j70, j45, j73, j71, j78, j94, j55, j41, j58, j9, k45, k43, k55, k36, k19, k87, k79, k33, k71, k23, k61, k99, k54, k41, k35, k7, k26, k18, k5, k13, k72, k73, k49, k30, k67, k78, k10, k93, k11, k12, k46, k56, k21, k62, k84, k58, k32, k51, l0, l7, l20, l92, l58, l12, l80, l94, l46, l48, l73, l25, l6, l62, l21, l52, l13, l19, l99, l57, l98, l53, l67, l41, l2, l27, l4, l47, l61, l71, l1, l85, l24, l29, l16, l56, l82, l81, l86, l35, l87, l50, l76, m37, m30, m56, m65, m25, m48, m46, m31, m50, m70, m71, m87, m3, m81, m52, m18, m63, m42, m6, m1, m39, m62, m15, m29, m78, m24, m10, m54, m88, m55, m23, m94, m21, m16, m12, m85, m41, m53, m11, n51, n99, n7, n86, n30, n74, n59, n73, n96, n11, n44, n68, n1, n98, n48, n32, n36, n22, n66, n88, n53, n93, n4, n13, n21, n69, n97, n26, n94, n82, n91, n61, n83, n71, n2, o67, o35, o29, o64, o66, o0, o13, o87, o47, o90, o55, o63, o69, o37, o2, o22, o54, o15, o16, o96, o75, o51, o89, o6, o73, o95, o79, o80, o14, o58, o92, o48, o59, p49, p32, p43, p10, p79, p3, p8, p24, p74, p33, p65, p86, p71, p75, p91, p17, p52, p13, p36, p7, p6, p39, p63, p4, p85, p57, p88, p31, p59, p15, p50, p28, p82, p9, p87, p64, p58, p95, p83, p46, q27, q31, q88, q93, q87, q13, q85, q22, q83, q30, q66, q79, q44, q36, q94, q71, q68, q28, q58, q12, q2, q18, q82, q34, q52, q55, q41, q5, q20, q43, q92, q53, r47, r67, r86, r66, r29, r23, r55, r39, r46, r71, r22, r53, r88, r63, r38, r7, r62, r83, r85, r80, r9, r81, r94, r15, r68, r8, r41, r13, r87, r0, r76, r30, r4, r43, r16, r95, r50, r92, r28, r37, r90, s66, s97, s47, s69, s95, s26, s24, s91, s7, s98, s48, s33, s31, s93, s19, s53, s37, s55, s96, s15, s6, s8, s83, s58, s25, s38, s5, s51, s68, s22, s14, s17, s40, t77, t7, t48, t61, t22, t17, t44, t35, t32, t81, t64, t71, t10, t72, t58, t25, t3, t70, t56, t24, t66, t30, t16, t2, t53, t6, t91, t23, t39, t49, t74, t67, t19, t45, t94, t69, t90, t0, t20, t78, t88, u0, u88, u79, u51, u87, u48, u66, u71, u64, u27, u3, u60, u58, u7, u94, u12, u97, u54, u57, u61, u21, u6, u89, u30, u32, u8, u75, u10, u46, u84, u77, u53, u95, u81, u73, u42, u93, u98, u59, u99, v35, v81, v93, v67, v54, v1, v15, v90, v82, v53, v42, v98, v22, v14, v77, v27, v16, v43, v26, v92, v74, v8, v59, v0, v71, v7, v72, v85, v5, v51, v79, v21, v33, v28, v44, v99, v88, v11, v2, v45, w64, w20, w67, w51, w30, w24, w92, w25, w90, w4, w98, w7, w60, w6, w88, w58, w27, w87, w28, w57, w72, w82, w78, w70, w66, w79, w11, w50, w76, w59, w42, w13, w44, w94, w84, w96, w22, w91, w32, w17, w36, x52, x59, x53, x62, x67, x21, x49, x33, x87, x44, x1, x70, x11, x74, x96, x77, x4, x0, x51, x16, x28, x27, x85, x75, x58, x99, x57, x93, x17, x29, x9, x26, x71, x35, x36, y11, y65, y50, y99, y69, y1, y79, y8, y61, y31, y23, y38, y90, y48, y46, y80, y97, y53, y17, y75, y26, y9, y4, y42, y89, y30, y47, y43, y88, y83, y25, y15, y86, y68, y2, y49, y92, y76, y35, y14, y63, z69, z56, z96, z16, z37, z1, z43, z97, z78, z29, z65, z38, z41, z21, z53, z80, z6, z45, z5, z82, z48, z75, z62, z31, z99, z27, z15, z63, z85, z35, z79, z71, z98, z4, z28, z54, z0, z86
L2: a98, a88, a86, a84, a80, a77, a76, a73, a70, a69, a67, a66, a64, a63, a62, a61, a57, a56, a51, a50, a49, a46, a45, a44, a42, a38, a36, a33, a31, a30, a28, a27, a24, a20, a10, a1, b97, b95, b94, b91, b89, b87, b86, b84, b83, b82, b77, b76, b71, b69, b68, b61, b60, b56, b54, b47, b43, b41, b40, b39, b36, b35, b33, b32, b30, b27, b22, b21, b18, b17, b14, b11, b9, b8, b2, b1, c99, c96, c94, c93, c90, c89, c88, c85, c81, c78, c71, c70, c69, c62, c61, c60, c54, c48, c47, c42, c41, c38, c36, c35, c34, c29, c25, c20, c19, c15, c13, c6, c5, c3, d99, d96, d95, d91, d90, d88, d87, d84, d81, d80, d73, d67, d66, d60, d59, d58, d56, d55, d54, d53, d43, d42, d41, d40, d35, d32, d29, d28, d27, d25, d24, d22, d21, d19, d18, d12, d11, d9, d7, d6, d4, d3, d2, e99, e94, e92, e91, e87, e84, e82, e80, e77, e76, e73, e67, e65, e64, e62, e56, e54, e46, e42, e41, e39, e38, e37, e36, e33, e32, e31, e30, e29, e28, e26, e25, e24, e19, e18, e16, e5, e4, e2, f96, f94, f91, f87, f84, f82, f76, f75, f73, f72, f68, f67, f66, f65, f64, f61, f57, f56, f47, f40, f37, f33, f31, f28, f27, f25, f24, f16, f12, f7, f3, g99, g97, g96, g95, g91, g90, g89, g85, g84, g82, g79, g70, g65, g64, g62, g61, g57, g53, g52, g50, g48, g47, g43, g40, g39, g38, g34, g31, g30, g28, g25, g23, g19, g16, g15, g12, g11, g9, g1, g0, h99, h95, h93, h91, h90, h89, h87, h85, h82, h79, h78, h76, h75, h74, h73, h72, h69, h67, h64, h61, h56, h55, h53, h51, h50, h47, h44, h40, h36, h33, h32, h29, h27, h26, h24, h18, h15, h12, h11, h7, h6, h5, h4, h0, i99, i97, i96, i94, i92, i87, i86, i85, i83, i81, i72, i71, i70, i69, i68, i67, i63, i59, i54, i53, i50, i49, i47, i37, i35, i34, i33, i29, i28, i23, i16, i14, i13, i10, i6, i2, j99, j94, j92, j84, j83, j82, j81, j80, j79, j78, j77, j76, j73, j72, j71, j70, j69, j65, j62, j58, j55, j54, j49, j47, j45, j43, j42, j41, j40, j38, j33, j32, j31, j29, j27, j25, j23, j22, j21, j19, j16, j15, j11, j9, j5, j2, j0, k99, k93, k87, k84, k79, k78, k73, k72, k71, k67, k62, k61, k58, k56, k55, k54, k51, k49, k46, k45, k43, k41, k36, k35, k33, k32, k30, k26, k23, k21, k19, k18, k13, k12, k11, k10, k7, k5, l99, l98, l94, l92, l87, l86, l85, l82, l81, l80, l76, l73, l71, l67, l62, l61, l58, l57, l56, l53, l52, l50, l48, l47, l46, l41, l35, l29, l27, l25, l24, l21, l20, l19, l16, l13, l12, l7, l6, l4, l2, l1, l0, m94, m88, m87, m85, m81, m78, m71, m70, m65, m63, m62, m56, m55, m54, m53, m52, m50, m48, m46, m42, m41, m39, m37, m31, m30, m29, m25, m24, m23, m21, m18, m16, m15, m12, m11, m10, m6, m3, m1, n99, n98, n97, n96, n94, n93, n91, n88, n86, n83, n82, n74, n73, n71, n69, n68, n66, n61, n59, n53, n51, n48, n44, n36, n32, n30, n26, n22, n21, n13, n11, n7, n4, n2, n1, o96, o95, o92, o90, o89, o87, o80, o79, o75, o73, o69, o67, o66, o64, o63, o59, o58, o55, o54, o51, o48, o47, o37, o35, o29, o22, o16, o15, o14, o13, o6, o2, o0, p95, p91, p88, p87, p86, p85, p83, p82, p79, p75, p74, p71, p65, p64, p63, p59, p58, p57, p52, p50, p49, p46, p43, p39, p36, p33, p32, p31, p28, p24, p17, p15, p13, p10, p9, p8, p7, p6, p4, p3, q94, q93, q92, q88, q87, q85, q83, q82, q79, q71, q68, q66, q58, q55, q53, q52, q44, q43, q41, q36, q34, q31, q30, q28, q27, q22, q20, q18, q13, q12, q5, q2, r95, r94, r92, r90, r88, r87, r86, r85, r83, r81, r80, r76, r71, r68, r67, r66, r63, r62, r55, r53, r50, r47, r46, r43, r41, r39, r38, r37, r30, r29, r28, r23, r22, r16, r15, r13, r9, r8, r7, r4, r0, s98, s97, s96, s95, s93, s91, s83, s69, s68, s66, s58, s55, s53, s51, s48, s47, s40, s38, s37, s33, s31, s26, s25, s24, s22, s19, s17, s15, s14, s8, s7, s6, s5, t94, t91, t90, t88, t81, t78, t77, t74, t72, t71, t70, t69, t67, t66, t64, t61, t58, t56, t53, t49, t48, t45, t44, t39, t35, t32, t30, t25, t24, t23, t22, t20, t19, t17, t16, t10, t7, t6, t3, t2, t0, u99, u98, u97, u95, u94, u93, u89, u88, u87, u84, u81, u79, u77, u75, u73, u71, u66, u64, u61, u60, u59, u58, u57, u54, u53, u51, u48, u46, u42, u32, u30, u27, u21, u12, u10, u8, u7, u6, u3, u0, v99, v98, v93, v92, v90, v88, v85, v82, v81, v79, v77, v74, v72, v71, v67, v59, v54, v53, v51, v45, v44, v43, v42, v35, v33, v28, v27, v26, v22, v21, v16, v15, v14, v11, v8, v7, v5, v2, v1, v0, w98, w96, w94, w92, w91, w90, w88, w87, w84, w82, w79, w78, w76, w72, w70, w67, w66, w64, w60, w59, w58, w57, w51, w50, w44, w42, w36, w32, w30, w28, w27, w25, w24, w22, w20, w17, w13, w11, w7, w6, w4, x99, x96, x93, x87, x85, x77, x75, x74, x71, x70, x67, x62, x59, x58, x57, x53, x52, x51, x49, x44, x36, x35, x33, x29, x28, x27, x26, x21, x17, x16, x11, x9, x4, x1, x0, y99, y97, y92, y90, y89, y88, y86, y83, y80, y79, y76, y75, y69, y68, y65, y63, y61, y53, y50, y49, y48, y47, y46, y43, y42, y38, y35, y31, y30, y26, y25, y23, y17, y15, y14, y11, y9, y8, y4, y2, y1, z99, z98, z97, z96, z86, z85, z82, z80, z79, z78, z75, z71, z69, z65, z63, z62, z56, z54, z53, z48, z45, z43, z41, z38, z37, z35, z31, z29, z28, z27, z21, z16, z15, z6, z5, z4, z1, z0
```

**Private test case 4**

**Input**

```
a0, a10, a12, a15, a17, a18, a2, a20, a24, a27, a29, a33, a35, a37, a39, a40, a43, a49, a5, a52, a53, a57, a58, a59, a6, a65, a67, a70, a74, a75, a77, a78, a8, a80, a82, a83, a85, a86, a9, a90, a92, a93, a95, a98, a99, b1, b14, b15, b16, b17, b2, b3, b31, b33, b34, b35, b36, b38, b42, b44, b46, b47, b48, b5, b51, b52, b53, b54, b58, b6, b60, b61, b63, b65, b67, b68, b69, b70, b74, b77, b80, b81, b85, b94, b98, c0, c10, c11, c12, c13, c14, c16, c17, c18, c20, c21, c23, c24, c25, c26, c27, c31, c32, c35, c4, c41, c42, c44, c45, c50, c53, c55, c57, c60, c63, c64, c66, c69, c7, c72, c73, c74, c75, c76, c77, c81, c84, c87, c92, c93, c97, c98, d11, d12, d13, d16, d18, d19, d2, d21, d24, d27, d28, d32, d36, d37, d4, d40, d45, d47, d52, d53, d55, d56, d58, d6, d66, d68, d69, d73, d74, d78, d81, d86, d87, d89, d9, d90, d94, d98, e0, e12, e16, e19, e2, e20, e26, e27, e28, e3, e30, e31, e32, e34, e36, e40, e42, e43, e44, e45, e46, e47, e48, e49, e50, e54, e55, e57, e58, e6, e62, e64, e66, e7, e71, e73, e75, e76, e78, e79, e81, e85, e86, e90, e92, e94, e95, e96, e99, f0, f11, f14, f15, f17, f18, f21, f22, f23, f24, f26, f27, f29, f3, f31, f34, f35, f36, f37, f41, f44, f45, f47, f49, f53, f54, f55, f58, f59, f6, f60, f61, f63, f64, f66, f69, f7, f70, f71, f72, f77, f78, f82, f83, f85, f86, f90, f91, f93, f94, f95, f96, f97, f98, g0, g1, g10, g12, g13, g14, g15, g18, g19, g21, g23, g24, g28, g3, g30, g33, g35, g37, g38, g39, g40, g42, g46, g48, g49, g55, g58, g59, g6, g61, g63, g7, g70, g77, g78, g84, g85, g88, g89, g91, g98, g99, h14, h17, h18, h2, h21, h23, h24, h25, h28, h29, h30, h32, h33, h34, h35, h36, h37, h38, h4, h41, h45, h46, h48, h49, h5, h50, h53, h54, h58, h59, h6, h61, h62, h64, h66, h67, h68, h75, h77, h78, h83, h94, h98, i0, i1, i10, i11, i12, i13, i17, i18, i19, i20, i21, i23, i29, i30, i33, i36, i38, i40, i41, i42, i46, i47, i48, i49, i53, i55, i58, i60, i61, i64, i65, i67, i70, i72, i75, i76, i77, i78, i80, i83, i9, i91, i94, i95, j10, j12, j15, j16, j18, j21, j22, j32, j33, j34, j35, j39, j41, j43, j45, j48, j49, j50, j51, j52, j56, j58, j59, j6, j61, j63, j7, j70, j73, j74, j75, j77, j79, j8, j80, j82, j83, j86, j87, j88, j89, j9, j90, j95, j99, k1, k10, k12, k14, k17, k18, k2, k20, k23, k25, k28, k34, k35, k36, k37, k39, k4, k40, k42, k43, k44, k45, k49, k5, k55, k58, k60, k62, k67, k68, k69, k7, k73, k74, k78, k80, k81, k83, k85, k88, k9, k92, k95, k97, k98, k99, l0, l1, l10, l12, l21, l25, l26, l29, l3, l32, l33, l35, l36, l38, l39, l41, l42, l43, l44, l47, l49, l50, l53, l54, l58, l6, l61, l63, l64, l66, l69, l7, l70, l71, l72, l73, l74, l8, l85, l86, l87, l88, l89, l90, l91, l92, l97, m0, m12, m13, m14, m18, m20, m22, m24, m26, m27, m29, m30, m31, m32, m33, m36, m37, m39, m40, m41, m42, m43, m44, m45, m5, m50, m53, m54, m57, m58, m60, m62, m63, m64, m65, m66, m67, m68, m69, m70, m73, m74, m76, m79, m8, m82, m84, m85, m88, m9, m90, m92, m94, m97, m99, n10, n11, n14, n19, n2, n23, n24, n25, n27, n3, n30, n33, n35, n37, n39, n41, n43, n46, n47, n50, n51, n52, n56, n57, n6, n65, n69, n7, n70, n72, n73, n74, n76, n79, n8, n80, n81, n82, n83, n84, n85, n89, n90, n92, n95, n96, n97, n98, n99, o0, o10, o12, o13, o14, o16, o20, o21, o23, o24, o25, o26, o27, o29, o3, o33, o36, o37, o38, o4, o41, o48, o49, o5, o50, o51, o53, o54, o55, o56, o57, o58, o60, o62, o63, o64, o66, o69, o75, o77, o79, o8, o81, o83, o85, o87, o92, o95, o96, o97, o98, o99, p0, p1, p10, p12, p14, p15, p16, p17, p19, p2, p21, p24, p26, p28, p29, p32, p33, p34, p40, p43, p44, p48, p49, p50, p52, p55, p56, p57, p6, p60, p61, p63, p64, p65, p66, p67, p68, p69, p70, p74, p76, p78, p79, p8, p82, p83, p86, p88, p92, p93, p94, p95, p96, p97, p99, q11, q14, q17, q19, q21, q24, q26, q27, q28, q29, q36, q37, q38, q39, q4, q41, q46, q47, q48, q49, q5, q50, q52, q54, q55, q57, q58, q6, q60, q61, q63, q65, q69, q70, q71, q76, q86, q88, q9, q92, q94, q97, q98, q99, r11, r12, r14, r15, r16, r19, r2, r22, r23, r24, r25, r27, r28, r3, r31, r36, r40, r41, r42, r43, r46, r47, r49, r50, r51, r54, r55, r57, r59, r6, r60, r61, r62, r65, r66, r69, r70, r71, r73, r74, r77, r8, r80, r85, r86, r87, r88, r89, r90, r91, r93, r94, r99, s0, s1, s14, s15, s16, s2, s20, s21, s22, s24, s25, s28, s29, s30, s32, s36, s38, s4, s40, s41, s46, s50, s53, s56, s58, s6, s62, s63, s67, s68, s69, s7, s71, s75, s77, s8, s81, s83, s87, s9, s90, s92, s93, s96, s98, t0, t1, t12, t13, t14, t18, t19, t2, t20, t23, t26, t28, t29, t30, t32, t33, t34, t35, t36, t37, t38, t42, t43, t44, t46, t5, t55, t56, t58, t6, t64, t67, t68, t70, t71, t73, t74, t76, t81, t86, t88, t92, t94, t96, u10, u11, u13, u16, u19, u22, u26, u28, u30, u31, u33, u34, u35, u40, u41, u44, u45, u46, u47, u48, u56, u57, u6, u60, u61, u62, u63, u66, u7, u73, u75, u77, u78, u79, u8, u80, u81, u84, u85, u87, u93, u94, u96, u99, v0, v1, v11, v13, v16, v21, v26, v28, v33, v36, v37, v41, v42, v46, v47, v48, v51, v52, v54, v55, v56, v57, v58, v59, v60, v61, v67, v7, v72, v73, v78, v80, v81, v82, v83, v85, v88, v9, v91, v95, v97, v98, w0, w11, w14, w15, w16, w17, w19, w2, w22, w24, w29, w30, w32, w34, w35, w39, w4, w41, w42, w43, w46, w47, w49, w51, w54, w57, w59, w61, w65, w67, w69, w70, w76, w77, w78, w8, w82, w85, w87, w89, w9, w94, w97, w98, x0, x1, x10, x13, x14, x16, x18, x2, x20, x23, x27, x28, x3, x31, x35, x37, x38, x39, x42, x44, x45, x47, x48, x50, x51, x54, x55, x57, x58, x59, x6, x60, x61, x64, x65, x68, x73, x75, x76, x77, x78, x81, x86, x87, x88, x90, x94, x95, x98, x99, y1, y12, y14, y15, y16, y17, y18, y21, y22, y27, y28, y29, y32, y33, y35, y36, y38, y4, y43, y47, y49, y50, y51, y55, y57, y62, y64, y67, y68, y70, y71, y74, y8, y80, y82, y84, y88, y9, y90, y91, y92, y96, y97, y98, y99, z1, z10, z11, z12, z14, z15, z18, z2, z22, z3, z32, z33, z35, z43, z44, z45, z49, z55, z58, z59, z6, z60, z62, z63, z64, z66, z67, z73, z76, z79, z8, z80, z82, z85, z89, z90, z92, z94
```

**Output**

```
L1: a0, a10, a12, a15, a17, a18, a2, a20, a24, a27, a29, a33, a35, a37, a39, a40, a43, a49, a5, a52, a53, a57, a58, a59, a6, a65, a67, a70, a74, a75, a77, a78, a8, a80, a82, a83, a85, a86, a9, a90, a92, a93, a95, a98, a99, b1, b14, b15, b16, b17, b2, b3, b31, b33, b34, b35, b36, b38, b42, b44, b46, b47, b48, b5, b51, b52, b53, b54, b58, b6, b60, b61, b63, b65, b67, b68, b69, b70, b74, b77, b80, b81, b85, b94, b98, c0, c10, c11, c12, c13, c14, c16, c17, c18, c20, c21, c23, c24, c25, c26, c27, c31, c32, c35, c4, c41, c42, c44, c45, c50, c53, c55, c57, c60, c63, c64, c66, c69, c7, c72, c73, c74, c75, c76, c77, c81, c84, c87, c92, c93, c97, c98, d11, d12, d13, d16, d18, d19, d2, d21, d24, d27, d28, d32, d36, d37, d4, d40, d45, d47, d52, d53, d55, d56, d58, d6, d66, d68, d69, d73, d74, d78, d81, d86, d87, d89, d9, d90, d94, d98, e0, e12, e16, e19, e2, e20, e26, e27, e28, e3, e30, e31, e32, e34, e36, e40, e42, e43, e44, e45, e46, e47, e48, e49, e50, e54, e55, e57, e58, e6, e62, e64, e66, e7, e71, e73, e75, e76, e78, e79, e81, e85, e86, e90, e92, e94, e95, e96, e99, f0, f11, f14, f15, f17, f18, f21, f22, f23, f24, f26, f27, f29, f3, f31, f34, f35, f36, f37, f41, f44, f45, f47, f49, f53, f54, f55, f58, f59, f6, f60, f61, f63, f64, f66, f69, f7, f70, f71, f72, f77, f78, f82, f83, f85, f86, f90, f91, f93, f94, f95, f96, f97, f98, g0, g1, g10, g12, g13, g14, g15, g18, g19, g21, g23, g24, g28, g3, g30, g33, g35, g37, g38, g39, g40, g42, g46, g48, g49, g55, g58, g59, g6, g61, g63, g7, g70, g77, g78, g84, g85, g88, g89, g91, g98, g99, h14, h17, h18, h2, h21, h23, h24, h25, h28, h29, h30, h32, h33, h34, h35, h36, h37, h38, h4, h41, h45, h46, h48, h49, h5, h50, h53, h54, h58, h59, h6, h61, h62, h64, h66, h67, h68, h75, h77, h78, h83, h94, h98, i0, i1, i10, i11, i12, i13, i17, i18, i19, i20, i21, i23, i29, i30, i33, i36, i38, i40, i41, i42, i46, i47, i48, i49, i53, i55, i58, i60, i61, i64, i65, i67, i70, i72, i75, i76, i77, i78, i80, i83, i9, i91, i94, i95, j10, j12, j15, j16, j18, j21, j22, j32, j33, j34, j35, j39, j41, j43, j45, j48, j49, j50, j51, j52, j56, j58, j59, j6, j61, j63, j7, j70, j73, j74, j75, j77, j79, j8, j80, j82, j83, j86, j87, j88, j89, j9, j90, j95, j99, k1, k10, k12, k14, k17, k18, k2, k20, k23, k25, k28, k34, k35, k36, k37, k39, k4, k40, k42, k43, k44, k45, k49, k5, k55, k58, k60, k62, k67, k68, k69, k7, k73, k74, k78, k80, k81, k83, k85, k88, k9, k92, k95, k97, k98, k99, l0, l1, l10, l12, l21, l25, l26, l29, l3, l32, l33, l35, l36, l38, l39, l41, l42, l43, l44, l47, l49, l50, l53, l54, l58, l6, l61, l63, l64, l66, l69, l7, l70, l71, l72, l73, l74, l8, l85, l86, l87, l88, l89, l90, l91, l92, l97, m0, m12, m13, m14, m18, m20, m22, m24, m26, m27, m29, m30, m31, m32, m33, m36, m37, m39, m40, m41, m42, m43, m44, m45, m5, m50, m53, m54, m57, m58, m60, m62, m63, m64, m65, m66, m67, m68, m69, m70, m73, m74, m76, m79, m8, m82, m84, m85, m88, m9, m90, m92, m94, m97, m99, n10, n11, n14, n19, n2, n23, n24, n25, n27, n3, n30, n33, n35, n37, n39, n41, n43, n46, n47, n50, n51, n52, n56, n57, n6, n65, n69, n7, n70, n72, n73, n74, n76, n79, n8, n80, n81, n82, n83, n84, n85, n89, n90, n92, n95, n96, n97, n98, n99, o0, o10, o12, o13, o14, o16, o20, o21, o23, o24, o25, o26, o27, o29, o3, o33, o36, o37, o38, o4, o41, o48, o49, o5, o50, o51, o53, o54, o55, o56, o57, o58, o60, o62, o63, o64, o66, o69, o75, o77, o79, o8, o81, o83, o85, o87, o92, o95, o96, o97, o98, o99, p0, p1, p10, p12, p14, p15, p16, p17, p19, p2, p21, p24, p26, p28, p29, p32, p33, p34, p40, p43, p44, p48, p49, p50, p52, p55, p56, p57, p6, p60, p61, p63, p64, p65, p66, p67, p68, p69, p70, p74, p76, p78, p79, p8, p82, p83, p86, p88, p92, p93, p94, p95, p96, p97, p99, q11, q14, q17, q19, q21, q24, q26, q27, q28, q29, q36, q37, q38, q39, q4, q41, q46, q47, q48, q49, q5, q50, q52, q54, q55, q57, q58, q6, q60, q61, q63, q65, q69, q70, q71, q76, q86, q88, q9, q92, q94, q97, q98, q99, r11, r12, r14, r15, r16, r19, r2, r22, r23, r24, r25, r27, r28, r3, r31, r36, r40, r41, r42, r43, r46, r47, r49, r50, r51, r54, r55, r57, r59, r6, r60, r61, r62, r65, r66, r69, r70, r71, r73, r74, r77, r8, r80, r85, r86, r87, r88, r89, r90, r91, r93, r94, r99, s0, s1, s14, s15, s16, s2, s20, s21, s22, s24, s25, s28, s29, s30, s32, s36, s38, s4, s40, s41, s46, s50, s53, s56, s58, s6, s62, s63, s67, s68, s69, s7, s71, s75, s77, s8, s81, s83, s87, s9, s90, s92, s93, s96, s98, t0, t1, t12, t13, t14, t18, t19, t2, t20, t23, t26, t28, t29, t30, t32, t33, t34, t35, t36, t37, t38, t42, t43, t44, t46, t5, t55, t56, t58, t6, t64, t67, t68, t70, t71, t73, t74, t76, t81, t86, t88, t92, t94, t96, u10, u11, u13, u16, u19, u22, u26, u28, u30, u31, u33, u34, u35, u40, u41, u44, u45, u46, u47, u48, u56, u57, u6, u60, u61, u62, u63, u66, u7, u73, u75, u77, u78, u79, u8, u80, u81, u84, u85, u87, u93, u94, u96, u99, v0, v1, v11, v13, v16, v21, v26, v28, v33, v36, v37, v41, v42, v46, v47, v48, v51, v52, v54, v55, v56, v57, v58, v59, v60, v61, v67, v7, v72, v73, v78, v80, v81, v82, v83, v85, v88, v9, v91, v95, v97, v98, w0, w11, w14, w15, w16, w17, w19, w2, w22, w24, w29, w30, w32, w34, w35, w39, w4, w41, w42, w43, w46, w47, w49, w51, w54, w57, w59, w61, w65, w67, w69, w70, w76, w77, w78, w8, w82, w85, w87, w89, w9, w94, w97, w98, x0, x1, x10, x13, x14, x16, x18, x2, x20, x23, x27, x28, x3, x31, x35, x37, x38, x39, x42, x44, x45, x47, x48, x50, x51, x54, x55, x57, x58, x59, x6, x60, x61, x64, x65, x68, x73, x75, x76, x77, x78, x81, x86, x87, x88, x90, x94, x95, x98, x99, y1, y12, y14, y15, y16, y17, y18, y21, y22, y27, y28, y29, y32, y33, y35, y36, y38, y4, y43, y47, y49, y50, y51, y55, y57, y62, y64, y67, y68, y70, y71, y74, y8, y80, y82, y84, y88, y9, y90, y91, y92, y96, y97, y98, y99, z1, z10, z11, z12, z14, z15, z18, z2, z22, z3, z32, z33, z35, z43, z44, z45, z49, z55, z58, z59, z6, z60, z62, z63, z64, z66, z67, z73, z76, z79, z8, z80, z82, z85, z89, z90, z92, z94
L2: a99, a98, a95, a93, a92, a90, a86, a85, a83, a82, a80, a78, a77, a75, a74, a70, a67, a65, a59, a58, a57, a53, a52, a49, a43, a40, a39, a37, a35, a33, a29, a27, a24, a20, a18, a17, a15, a12, a10, a9, a8, a6, a5, a2, a0, b98, b94, b85, b81, b80, b77, b74, b70, b69, b68, b67, b65, b63, b61, b60, b58, b54, b53, b52, b51, b48, b47, b46, b44, b42, b38, b36, b35, b34, b33, b31, b17, b16, b15, b14, b6, b5, b3, b2, b1, c98, c97, c93, c92, c87, c84, c81, c77, c76, c75, c74, c73, c72, c69, c66, c64, c63, c60, c57, c55, c53, c50, c45, c44, c42, c41, c35, c32, c31, c27, c26, c25, c24, c23, c21, c20, c18, c17, c16, c14, c13, c12, c11, c10, c7, c4, c0, d98, d94, d90, d89, d87, d86, d81, d78, d74, d73, d69, d68, d66, d58, d56, d55, d53, d52, d47, d45, d40, d37, d36, d32, d28, d27, d24, d21, d19, d18, d16, d13, d12, d11, d9, d6, d4, d2, e99, e96, e95, e94, e92, e90, e86, e85, e81, e79, e78, e76, e75, e73, e71, e66, e64, e62, e58, e57, e55, e54, e50, e49, e48, e47, e46, e45, e44, e43, e42, e40, e36, e34, e32, e31, e30, e28, e27, e26, e20, e19, e16, e12, e7, e6, e3, e2, e0, f98, f97, f96, f95, f94, f93, f91, f90, f86, f85, f83, f82, f78, f77, f72, f71, f70, f69, f66, f64, f63, f61, f60, f59, f58, f55, f54, f53, f49, f47, f45, f44, f41, f37, f36, f35, f34, f31, f29, f27, f26, f24, f23, f22, f21, f18, f17, f15, f14, f11, f7, f6, f3, f0, g99, g98, g91, g89, g88, g85, g84, g78, g77, g70, g63, g61, g59, g58, g55, g49, g48, g46, g42, g40, g39, g38, g37, g35, g33, g30, g28, g24, g23, g21, g19, g18, g15, g14, g13, g12, g10, g7, g6, g3, g1, g0, h98, h94, h83, h78, h77, h75, h68, h67, h66, h64, h62, h61, h59, h58, h54, h53, h50, h49, h48, h46, h45, h41, h38, h37, h36, h35, h34, h33, h32, h30, h29, h28, h25, h24, h23, h21, h18, h17, h14, h6, h5, h4, h2, i95, i94, i91, i83, i80, i78, i77, i76, i75, i72, i70, i67, i65, i64, i61, i60, i58, i55, i53, i49, i48, i47, i46, i42, i41, i40, i38, i36, i33, i30, i29, i23, i21, i20, i19, i18, i17, i13, i12, i11, i10, i9, i1, i0, j99, j95, j90, j89, j88, j87, j86, j83, j82, j80, j79, j77, j75, j74, j73, j70, j63, j61, j59, j58, j56, j52, j51, j50, j49, j48, j45, j43, j41, j39, j35, j34, j33, j32, j22, j21, j18, j16, j15, j12, j10, j9, j8, j7, j6, k99, k98, k97, k95, k92, k88, k85, k83, k81, k80, k78, k74, k73, k69, k68, k67, k62, k60, k58, k55, k49, k45, k44, k43, k42, k40, k39, k37, k36, k35, k34, k28, k25, k23, k20, k18, k17, k14, k12, k10, k9, k7, k5, k4, k2, k1, l97, l92, l91, l90, l89, l88, l87, l86, l85, l74, l73, l72, l71, l70, l69, l66, l64, l63, l61, l58, l54, l53, l50, l49, l47, l44, l43, l42, l41, l39, l38, l36, l35, l33, l32, l29, l26, l25, l21, l12, l10, l8, l7, l6, l3, l1, l0, m99, m97, m94, m92, m90, m88, m85, m84, m82, m79, m76, m74, m73, m70, m69, m68, m67, m66, m65, m64, m63, m62, m60, m58, m57, m54, m53, m50, m45, m44, m43, m42, m41, m40, m39, m37, m36, m33, m32, m31, m30, m29, m27, m26, m24, m22, m20, m18, m14, m13, m12, m9, m8, m5, m0, n99, n98, n97, n96, n95, n92, n90, n89, n85, n84, n83, n82, n81, n80, n79, n76, n74, n73, n72, n70, n69, n65, n57, n56, n52, n51, n50, n47, n46, n43, n41, n39, n37, n35, n33, n30, n27, n25, n24, n23, n19, n14, n11, n10, n8, n7, n6, n3, n2, o99, o98, o97, o96, o95, o92, o87, o85, o83, o81, o79, o77, o75, o69, o66, o64, o63, o62, o60, o58, o57, o56, o55, o54, o53, o51, o50, o49, o48, o41, o38, o37, o36, o33, o29, o27, o26, o25, o24, o23, o21, o20, o16, o14, o13, o12, o10, o8, o5, o4, o3, o0, p99, p97, p96, p95, p94, p93, p92, p88, p86, p83, p82, p79, p78, p76, p74, p70, p69, p68, p67, p66, p65, p64, p63, p61, p60, p57, p56, p55, p52, p50, p49, p48, p44, p43, p40, p34, p33, p32, p29, p28, p26, p24, p21, p19, p17, p16, p15, p14, p12, p10, p8, p6, p2, p1, p0, q99, q98, q97, q94, q92, q88, q86, q76, q71, q70, q69, q65, q63, q61, q60, q58, q57, q55, q54, q52, q50, q49, q48, q47, q46, q41, q39, q38, q37, q36, q29, q28, q27, q26, q24, q21, q19, q17, q14, q11, q9, q6, q5, q4, r99, r94, r93, r91, r90, r89, r88, r87, r86, r85, r80, r77, r74, r73, r71, r70, r69, r66, r65, r62, r61, r60, r59, r57, r55, r54, r51, r50, r49, r47, r46, r43, r42, r41, r40, r36, r31, r28, r27, r25, r24, r23, r22, r19, r16, r15, r14, r12, r11, r8, r6, r3, r2, s98, s96, s93, s92, s90, s87, s83, s81, s77, s75, s71, s69, s68, s67, s63, s62, s58, s56, s53, s50, s46, s41, s40, s38, s36, s32, s30, s29, s28, s25, s24, s22, s21, s20, s16, s15, s14, s9, s8, s7, s6, s4, s2, s1, s0, t96, t94, t92, t88, t86, t81, t76, t74, t73, t71, t70, t68, t67, t64, t58, t56, t55, t46, t44, t43, t42, t38, t37, t36, t35, t34, t33, t32, t30, t29, t28, t26, t23, t20, t19, t18, t14, t13, t12, t6, t5, t2, t1, t0, u99, u96, u94, u93, u87, u85, u84, u81, u80, u79, u78, u77, u75, u73, u66, u63, u62, u61, u60, u57, u56, u48, u47, u46, u45, u44, u41, u40, u35, u34, u33, u31, u30, u28, u26, u22, u19, u16, u13, u11, u10, u8, u7, u6, v98, v97, v95, v91, v88, v85, v83, v82, v81, v80, v78, v73, v72, v67, v61, v60, v59, v58, v57, v56, v55, v54, v52, v51, v48, v47, v46, v42, v41, v37, v36, v33, v28, v26, v21, v16, v13, v11, v9, v7, v1, v0, w98, w97, w94, w89, w87, w85, w82, w78, w77, w76, w70, w69, w67, w65, w61, w59, w57, w54, w51, w49, w47, w46, w43, w42, w41, w39, w35, w34, w32, w30, w29, w24, w22, w19, w17, w16, w15, w14, w11, w9, w8, w4, w2, w0, x99, x98, x95, x94, x90, x88, x87, x86, x81, x78, x77, x76, x75, x73, x68, x65, x64, x61, x60, x59, x58, x57, x55, x54, x51, x50, x48, x47, x45, x44, x42, x39, x38, x37, x35, x31, x28, x27, x23, x20, x18, x16, x14, x13, x10, x6, x3, x2, x1, x0, y99, y98, y97, y96, y92, y91, y90, y88, y84, y82, y80, y74, y71, y70, y68, y67, y64, y62, y57, y55, y51, y50, y49, y47, y43, y38, y36, y35, y33, y32, y29, y28, y27, y22, y21, y18, y17, y16, y15, y14, y12, y9, y8, y4, y1, z94, z92, z90, z89, z85, z82, z80, z79, z76, z73, z67, z66, z64, z63, z62, z60, z59, z58, z55, z49, z45, z44, z43, z35, z33, z32, z22, z18, z15, z14, z12, z11, z10, z8, z6, z3, z2, z1
```

**Private test case 5**

**Input**

```
a0, a10, a13, a17, a18, a19, a2, a21, a24, a26, a28, a29, a3, a30, a33, a34, a36, a38, a40, a42, a45, a49, a5, a50, a51, a52, a53, a54, a55, a58, a59, a60, a61, a63, a64, a65, a67, a68, a69, a7, a70, a73, a74, a76, a77, a78, a79, a8, a81, a85, a86, a87, a88, a89, a90, a91, a92, a93, a94, a95, a96, a97, a98, a99, b0, b1, b10, b11, b13, b14, b15, b18, b19, b2, b20, b21, b23, b24, b25, b26, b27, b28, b29, b3, b30, b33, b35, b40, b44, b45, b47, b48, b49, b5, b50, b52, b53, b56, b57, b58, b59, b6, b61, b62, b64, b66, b67, b69, b7, b70, b72, b73, b74, b75, b76, b77, b78, b8, b80, b82, b84, b85, b88, b90, b91, b92, b94, b98, b99, c0, c1, c12, c13, c14, c16, c17, c18, c20, c25, c26, c27, c3, c31, c32, c34, c37, c39, c4, c41, c42, c44, c45, c46, c47, c48, c49, c50, c54, c55, c56, c58, c59, c6, c60, c62, c65, c67, c68, c69, c7, c71, c72, c74, c75, c78, c82, c83, c84, c85, c86, c87, c88, c89, c91, c96, c98, d0, d1, d10, d11, d12, d13, d14, d15, d16, d17, d18, d19, d20, d21, d22, d23, d27, d28, d30, d31, d32, d33, d36, d37, d38, d40, d41, d42, d43, d44, d47, d48, d49, d5, d52, d53, d54, d55, d56, d57, d58, d6, d60, d62, d65, d68, d69, d7, d71, d72, d73, d76, d77, d78, d79, d8, d80, d82, d83, d84, d87, d88, d89, d9, d90, d91, d92, d94, d95, d96, e1, e11, e15, e18, e19, e2, e21, e22, e23, e24, e25, e26, e27, e28, e29, e3, e30, e33, e34, e35, e36, e39, e4, e40, e41, e43, e44, e45, e46, e47, e5, e50, e56, e57, e58, e59, e6, e60, e62, e63, e64, e67, e69, e7, e70, e72, e73, e74, e75, e78, e8, e80, e82, e83, e84, e86, e88, e9, e90, e91, e93, e94, e95, e98, e99, f0, f1, f10, f15, f17, f18, f19, f2, f20, f21, f22, f24, f27, f29, f30, f31, f32, f33, f34, f35, f38, f40, f42, f45, f48, f50, f52, f55, f56, f58, f59, f6, f60, f61, f62, f63, f64, f66, f68, f69, f7, f71, f72, f74, f75, f76, f77, f8, f80, f82, f83, f84, f85, f86, f88, f89, f9, f96, f97, f98, f99, g0, g1, g10, g11, g14, g16, g17, g18, g22, g25, g27, g28, g29, g3, g30, g32, g33, g35, g36, g37, g38, g39, g41, g46, g49, g50, g52, g54, g55, g57, g58, g6, g61, g62, g64, g65, g66, g67, g68, g7, g70, g72, g75, g78, g80, g84, g88, g9, g92, g93, g94, g96, g97, g98, h1, h10, h11, h12, h13, h15, h17, h18, h2, h20, h23, h24, h25, h26, h27, h29, h3, h31, h34, h36, h37, h4, h40, h41, h42, h43, h45, h46, h47, h48, h5, h52, h54, h56, h57, h58, h59, h60, h61, h62, h63, h64, h65, h66, h69, h7, h72, h74, h77, h78, h79, h8, h80, h81, h84, h86, h89, h90, h92, h94, h97, i0, i1, i10, i12, i13, i14, i15, i16, i18, i19, i2, i20, i21, i22, i23, i27, i28, i3, i30, i31, i32, i39, i40, i41, i42, i43, i54, i55, i57, i58, i60, i62, i63, i64, i66, i69, i7, i70, i71, i72, i73, i75, i76, i77, i78, i8, i84, i86, i87, i89, i93, i94, i96, i97, i98, i99, j0, j1, j10, j11, j14, j15, j17, j18, j19, j22, j23, j25, j26, j27, j28, j3, j31, j32, j35, j39, j40, j43, j44, j45, j46, j47, j48, j49, j50, j52, j54, j56, j58, j59, j6, j60, j61, j62, j64, j65, j67, j68, j69, j7, j71, j72, j73, j74, j75, j76, j78, j79, j8, j81, j83, j84, j85, j87, j89, j90, j91, j92, j93, j96, j97, j98, j99, k0, k1, k10, k16, k17, k18, k19, k2, k21, k22, k23, k24, k26, k29, k30, k32, k33, k35, k37, k38, k39, k40, k41, k42, k43, k45, k47, k48, k49, k5, k50, k51, k52, k54, k55, k58, k59, k6, k60, k61, k62, k64, k65, k66, k68, k69, k7, k70, k71, k73, k78, k8, k80, k81, k84, k86, k87, k89, k9, k90, k92, k93, k94, k96, k97, k99, l0, l1, l11, l12, l13, l14, l15, l17, l19, l2, l20, l22, l24, l25, l28, l30, l31, l32, l34, l39, l4, l40, l42, l44, l46, l47, l49, l50, l52, l53, l54, l55, l56, l59, l6, l60, l62, l64, l66, l67, l68, l69, l70, l72, l74, l76, l77, l78, l8, l81, l82, l83, l85, l87, l88, l89, l9, l90, l92, l93, l94, l95, l96, l97, l98, m1, m11, m13, m14, m15, m16, m17, m19, m20, m22, m23, m24, m26, m29, m3, m31, m32, m34, m37, m38, m39, m4, m40, m41, m42, m43, m44, m46, m49, m50, m51, m52, m53, m54, m55, m56, m57, m6, m60, m61, m62, m64, m65, m66, m67, m70, m71, m73, m75, m78, m79, m80, m81, m82, m90, m94, m95, m96, m99, n0, n1, n10, n11, n13, n14, n15, n17, n18, n19, n20, n21, n23, n24, n25, n26, n28, n33, n34, n37, n39, n41, n42, n43, n44, n45, n46, n47, n49, n5, n50, n52, n53, n54, n55, n57, n58, n59, n61, n62, n64, n65, n67, n68, n7, n70, n72, n73, n75, n76, n77, n80, n83, n84, n85, n86, n87, n88, n89, n9, n90, n92, n95, n96, n97, n98, o0, o1, o10, o14, o16, o17, o18, o19, o2, o20, o21, o23, o24, o25, o28, o29, o3, o36, o37, o38, o39, o4, o40, o41, o42, o44, o48, o5, o50, o51, o52, o54, o55, o59, o6, o60, o61, o62, o64, o65, o67, o68, o69, o7, o71, o74, o76, o79, o8, o83, o84, o85, o88, o9, o90, o93, o94, o97, o98, p1, p10, p11, p12, p13, p14, p18, p19, p2, p20, p21, p23, p24, p25, p26, p27, p28, p29, p3, p32, p34, p36, p37, p38, p44, p46, p47, p48, p50, p51, p53, p54, p55, p6, p60, p63, p7, p71, p74, p76, p78, p8, p80, p81, p83, p86, p87, p88, p89, p9, p92, p94, p95, p96, p98, q0, q1, q10, q11, q12, q14, q16, q17, q18, q2, q20, q21, q23, q24, q26, q27, q28, q29, q3, q30, q31, q32, q33, q34, q36, q38, q39, q4, q42, q45, q48, q49, q51, q52, q54, q55, q56, q59, q6, q60, q62, q63, q64, q68, q72, q78, q8, q80, q81, q83, q85, q86, q87, q88, q9, q90, q91, q92, q93, q96, q97, q98, q99, r10, r12, r13, r15, r19, r2, r20, r21, r22, r24, r25, r3, r30, r32, r34, r35, r36, r37, r38, r39, r4, r40, r42, r43, r44, r45, r46, r47, r48, r49, r50, r51, r55, r56, r58, r59, r60, r63, r64, r66, r67, r68, r69, r71, r72, r73, r74, r76, r77, r8, r80, r81, r82, r83, r84, r88, r89, r9, r90, r91, r92, r93, r94, r97, r99, s0, s11, s13, s14, s15, s2, s22, s23, s24, s25, s26, s27, s28, s30, s33, s34, s35, s36, s37, s39, s4, s40, s41, s42, s46, s47, s48, s49, s51, s53, s55, s56, s57, s59, s61, s63, s64, s65, s67, s68, s7, s71, s72, s73, s74, s75, s77, s8, s82, s83, s86, s88, s9, s90, s91, s92, s97, s98, s99, t0, t11, t16, t20, t21, t22, t23, t25, t26, t29, t3, t30, t33, t34, t35, t36, t37, t38, t39, t40, t41, t42, t45, t47, t48, t49, t50, t52, t53, t55, t57, t58, t62, t65, t67, t69, t70, t71, t73, t77, t79, t80, t81, t83, t84, t85, t86, t88, t89, t9, t90, t92, t95, t96, t98, t99, u11, u15, u17, u21, u22, u24, u27, u28, u29, u3, u30, u32, u35, u36, u37, u39, u40, u41, u43, u44, u45, u47, u48, u5, u50, u51, u52, u53, u56, u57, u58, u59, u60, u61, u62, u64, u65, u66, u68, u7, u71, u72, u73, u74, u76, u77, u78, u79, u8, u80, u81, u82, u83, u84, u86, u87, u88, u89, u9, u91, u94, v11, v13, v14, v17, v18, v19, v2, v20, v25, v26, v27, v28, v29, v3, v31, v32, v33, v34, v35, v37, v4, v40, v43, v45, v47, v5, v52, v54, v55, v56, v58, v59, v62, v64, v65, v66, v68, v69, v7, v71, v72, v73, v74, v75, v76, v78, v79, v80, v81, v84, v85, v86, v88, v89, v92, v93, v94, v95, v97, v99, w0, w1, w12, w14, w16, w18, w19, w2, w22, w23, w25, w27, w28, w29, w3, w30, w31, w32, w34, w35, w36, w37, w39, w40, w41, w42, w43, w44, w47, w48, w5, w50, w54, w55, w57, w59, w62, w63, w64, w66, w68, w69, w7, w70, w71, w75, w79, w81, w83, w84, w86, w87, w9, w91, w92, w95, w96, x10, x13, x14, x15, x16, x17, x18, x2, x20, x21, x23, x24, x25, x26, x27, x28, x29, x31, x32, x33, x34, x35, x36, x37, x4, x40, x42, x43, x44, x45, x46, x49, x5, x52, x53, x54, x56, x60, x61, x62, x63, x64, x65, x68, x69, x7, x74, x75, x76, x77, x80, x83, x84, x85, x88, x94, x95, x96, x98, x99, y1, y10, y12, y13, y14, y15, y16, y18, y21, y25, y26, y27, y28, y29, y3, y31, y32, y33, y35, y39, y41, y42, y43, y45, y46, y47, y48, y49, y50, y51, y52, y55, y56, y59, y6, y62, y63, y64, y65, y66, y67, y68, y69, y7, y71, y72, y73, y75, y76, y77, y78, y79, y8, y84, y86, y87, y88, y89, y9, y90, y91, y97, y98, y99, z0, z1, z10, z12, z13, z15, z17, z18, z19, z2, z21, z22, z24, z25, z26, z27, z3, z33, z35, z36, z37, z38, z39, z4, z41, z44, z46, z47, z5, z50, z52, z53, z54, z57, z58, z59, z6, z61, z64, z65, z66, z68, z69, z72, z73, z75, z76, z77, z79, z8, z80, z82, z83, z84, z85, z87, z89, z9, z90, z92, z93, z94, z95, z96, z98
```

**Output**

```
L1: a0, a10, a13, a17, a18, a19, a2, a21, a24, a26, a28, a29, a3, a30, a33, a34, a36, a38, a40, a42, a45, a49, a5, a50, a51, a52, a53, a54, a55, a58, a59, a60, a61, a63, a64, a65, a67, a68, a69, a7, a70, a73, a74, a76, a77, a78, a79, a8, a81, a85, a86, a87, a88, a89, a90, a91, a92, a93, a94, a95, a96, a97, a98, a99, b0, b1, b10, b11, b13, b14, b15, b18, b19, b2, b20, b21, b23, b24, b25, b26, b27, b28, b29, b3, b30, b33, b35, b40, b44, b45, b47, b48, b49, b5, b50, b52, b53, b56, b57, b58, b59, b6, b61, b62, b64, b66, b67, b69, b7, b70, b72, b73, b74, b75, b76, b77, b78, b8, b80, b82, b84, b85, b88, b90, b91, b92, b94, b98, b99, c0, c1, c12, c13, c14, c16, c17, c18, c20, c25, c26, c27, c3, c31, c32, c34, c37, c39, c4, c41, c42, c44, c45, c46, c47, c48, c49, c50, c54, c55, c56, c58, c59, c6, c60, c62, c65, c67, c68, c69, c7, c71, c72, c74, c75, c78, c82, c83, c84, c85, c86, c87, c88, c89, c91, c96, c98, d0, d1, d10, d11, d12, d13, d14, d15, d16, d17, d18, d19, d20, d21, d22, d23, d27, d28, d30, d31, d32, d33, d36, d37, d38, d40, d41, d42, d43, d44, d47, d48, d49, d5, d52, d53, d54, d55, d56, d57, d58, d6, d60, d62, d65, d68, d69, d7, d71, d72, d73, d76, d77, d78, d79, d8, d80, d82, d83, d84, d87, d88, d89, d9, d90, d91, d92, d94, d95, d96, e1, e11, e15, e18, e19, e2, e21, e22, e23, e24, e25, e26, e27, e28, e29, e3, e30, e33, e34, e35, e36, e39, e4, e40, e41, e43, e44, e45, e46, e47, e5, e50, e56, e57, e58, e59, e6, e60, e62, e63, e64, e67, e69, e7, e70, e72, e73, e74, e75, e78, e8, e80, e82, e83, e84, e86, e88, e9, e90, e91, e93, e94, e95, e98, e99, f0, f1, f10, f15, f17, f18, f19, f2, f20, f21, f22, f24, f27, f29, f30, f31, f32, f33, f34, f35, f38, f40, f42, f45, f48, f50, f52, f55, f56, f58, f59, f6, f60, f61, f62, f63, f64, f66, f68, f69, f7, f71, f72, f74, f75, f76, f77, f8, f80, f82, f83, f84, f85, f86, f88, f89, f9, f96, f97, f98, f99, g0, g1, g10, g11, g14, g16, g17, g18, g22, g25, g27, g28, g29, g3, g30, g32, g33, g35, g36, g37, g38, g39, g41, g46, g49, g50, g52, g54, g55, g57, g58, g6, g61, g62, g64, g65, g66, g67, g68, g7, g70, g72, g75, g78, g80, g84, g88, g9, g92, g93, g94, g96, g97, g98, h1, h10, h11, h12, h13, h15, h17, h18, h2, h20, h23, h24, h25, h26, h27, h29, h3, h31, h34, h36, h37, h4, h40, h41, h42, h43, h45, h46, h47, h48, h5, h52, h54, h56, h57, h58, h59, h60, h61, h62, h63, h64, h65, h66, h69, h7, h72, h74, h77, h78, h79, h8, h80, h81, h84, h86, h89, h90, h92, h94, h97, i0, i1, i10, i12, i13, i14, i15, i16, i18, i19, i2, i20, i21, i22, i23, i27, i28, i3, i30, i31, i32, i39, i40, i41, i42, i43, i54, i55, i57, i58, i60, i62, i63, i64, i66, i69, i7, i70, i71, i72, i73, i75, i76, i77, i78, i8, i84, i86, i87, i89, i93, i94, i96, i97, i98, i99, j0, j1, j10, j11, j14, j15, j17, j18, j19, j22, j23, j25, j26, j27, j28, j3, j31, j32, j35, j39, j40, j43, j44, j45, j46, j47, j48, j49, j50, j52, j54, j56, j58, j59, j6, j60, j61, j62, j64, j65, j67, j68, j69, j7, j71, j72, j73, j74, j75, j76, j78, j79, j8, j81, j83, j84, j85, j87, j89, j90, j91, j92, j93, j96, j97, j98, j99, k0, k1, k10, k16, k17, k18, k19, k2, k21, k22, k23, k24, k26, k29, k30, k32, k33, k35, k37, k38, k39, k40, k41, k42, k43, k45, k47, k48, k49, k5, k50, k51, k52, k54, k55, k58, k59, k6, k60, k61, k62, k64, k65, k66, k68, k69, k7, k70, k71, k73, k78, k8, k80, k81, k84, k86, k87, k89, k9, k90, k92, k93, k94, k96, k97, k99, l0, l1, l11, l12, l13, l14, l15, l17, l19, l2, l20, l22, l24, l25, l28, l30, l31, l32, l34, l39, l4, l40, l42, l44, l46, l47, l49, l50, l52, l53, l54, l55, l56, l59, l6, l60, l62, l64, l66, l67, l68, l69, l70, l72, l74, l76, l77, l78, l8, l81, l82, l83, l85, l87, l88, l89, l9, l90, l92, l93, l94, l95, l96, l97, l98, m1, m11, m13, m14, m15, m16, m17, m19, m20, m22, m23, m24, m26, m29, m3, m31, m32, m34, m37, m38, m39, m4, m40, m41, m42, m43, m44, m46, m49, m50, m51, m52, m53, m54, m55, m56, m57, m6, m60, m61, m62, m64, m65, m66, m67, m70, m71, m73, m75, m78, m79, m80, m81, m82, m90, m94, m95, m96, m99, n0, n1, n10, n11, n13, n14, n15, n17, n18, n19, n20, n21, n23, n24, n25, n26, n28, n33, n34, n37, n39, n41, n42, n43, n44, n45, n46, n47, n49, n5, n50, n52, n53, n54, n55, n57, n58, n59, n61, n62, n64, n65, n67, n68, n7, n70, n72, n73, n75, n76, n77, n80, n83, n84, n85, n86, n87, n88, n89, n9, n90, n92, n95, n96, n97, n98, o0, o1, o10, o14, o16, o17, o18, o19, o2, o20, o21, o23, o24, o25, o28, o29, o3, o36, o37, o38, o39, o4, o40, o41, o42, o44, o48, o5, o50, o51, o52, o54, o55, o59, o6, o60, o61, o62, o64, o65, o67, o68, o69, o7, o71, o74, o76, o79, o8, o83, o84, o85, o88, o9, o90, o93, o94, o97, o98, p1, p10, p11, p12, p13, p14, p18, p19, p2, p20, p21, p23, p24, p25, p26, p27, p28, p29, p3, p32, p34, p36, p37, p38, p44, p46, p47, p48, p50, p51, p53, p54, p55, p6, p60, p63, p7, p71, p74, p76, p78, p8, p80, p81, p83, p86, p87, p88, p89, p9, p92, p94, p95, p96, p98, q0, q1, q10, q11, q12, q14, q16, q17, q18, q2, q20, q21, q23, q24, q26, q27, q28, q29, q3, q30, q31, q32, q33, q34, q36, q38, q39, q4, q42, q45, q48, q49, q51, q52, q54, q55, q56, q59, q6, q60, q62, q63, q64, q68, q72, q78, q8, q80, q81, q83, q85, q86, q87, q88, q9, q90, q91, q92, q93, q96, q97, q98, q99, r10, r12, r13, r15, r19, r2, r20, r21, r22, r24, r25, r3, r30, r32, r34, r35, r36, r37, r38, r39, r4, r40, r42, r43, r44, r45, r46, r47, r48, r49, r50, r51, r55, r56, r58, r59, r60, r63, r64, r66, r67, r68, r69, r71, r72, r73, r74, r76, r77, r8, r80, r81, r82, r83, r84, r88, r89, r9, r90, r91, r92, r93, r94, r97, r99, s0, s11, s13, s14, s15, s2, s22, s23, s24, s25, s26, s27, s28, s30, s33, s34, s35, s36, s37, s39, s4, s40, s41, s42, s46, s47, s48, s49, s51, s53, s55, s56, s57, s59, s61, s63, s64, s65, s67, s68, s7, s71, s72, s73, s74, s75, s77, s8, s82, s83, s86, s88, s9, s90, s91, s92, s97, s98, s99, t0, t11, t16, t20, t21, t22, t23, t25, t26, t29, t3, t30, t33, t34, t35, t36, t37, t38, t39, t40, t41, t42, t45, t47, t48, t49, t50, t52, t53, t55, t57, t58, t62, t65, t67, t69, t70, t71, t73, t77, t79, t80, t81, t83, t84, t85, t86, t88, t89, t9, t90, t92, t95, t96, t98, t99, u11, u15, u17, u21, u22, u24, u27, u28, u29, u3, u30, u32, u35, u36, u37, u39, u40, u41, u43, u44, u45, u47, u48, u5, u50, u51, u52, u53, u56, u57, u58, u59, u60, u61, u62, u64, u65, u66, u68, u7, u71, u72, u73, u74, u76, u77, u78, u79, u8, u80, u81, u82, u83, u84, u86, u87, u88, u89, u9, u91, u94, v11, v13, v14, v17, v18, v19, v2, v20, v25, v26, v27, v28, v29, v3, v31, v32, v33, v34, v35, v37, v4, v40, v43, v45, v47, v5, v52, v54, v55, v56, v58, v59, v62, v64, v65, v66, v68, v69, v7, v71, v72, v73, v74, v75, v76, v78, v79, v80, v81, v84, v85, v86, v88, v89, v92, v93, v94, v95, v97, v99, w0, w1, w12, w14, w16, w18, w19, w2, w22, w23, w25, w27, w28, w29, w3, w30, w31, w32, w34, w35, w36, w37, w39, w40, w41, w42, w43, w44, w47, w48, w5, w50, w54, w55, w57, w59, w62, w63, w64, w66, w68, w69, w7, w70, w71, w75, w79, w81, w83, w84, w86, w87, w9, w91, w92, w95, w96, x10, x13, x14, x15, x16, x17, x18, x2, x20, x21, x23, x24, x25, x26, x27, x28, x29, x31, x32, x33, x34, x35, x36, x37, x4, x40, x42, x43, x44, x45, x46, x49, x5, x52, x53, x54, x56, x60, x61, x62, x63, x64, x65, x68, x69, x7, x74, x75, x76, x77, x80, x83, x84, x85, x88, x94, x95, x96, x98, x99, y1, y10, y12, y13, y14, y15, y16, y18, y21, y25, y26, y27, y28, y29, y3, y31, y32, y33, y35, y39, y41, y42, y43, y45, y46, y47, y48, y49, y50, y51, y52, y55, y56, y59, y6, y62, y63, y64, y65, y66, y67, y68, y69, y7, y71, y72, y73, y75, y76, y77, y78, y79, y8, y84, y86, y87, y88, y89, y9, y90, y91, y97, y98, y99, z0, z1, z10, z12, z13, z15, z17, z18, z19, z2, z21, z22, z24, z25, z26, z27, z3, z33, z35, z36, z37, z38, z39, z4, z41, z44, z46, z47, z5, z50, z52, z53, z54, z57, z58, z59, z6, z61, z64, z65, z66, z68, z69, z72, z73, z75, z76, z77, z79, z8, z80, z82, z83, z84, z85, z87, z89, z9, z90, z92, z93, z94, z95, z96, z98
L2: a99, a98, a97, a96, a95, a94, a93, a92, a91, a90, a89, a88, a87, a86, a85, a81, a79, a78, a77, a76, a74, a73, a70, a69, a68, a67, a65, a64, a63, a61, a60, a59, a58, a55, a54, a53, a52, a51, a50, a49, a45, a42, a40, a38, a36, a34, a33, a30, a29, a28, a26, a24, a21, a19, a18, a17, a13, a10, a8, a7, a5, a3, a2, a0, b99, b98, b94, b92, b91, b90, b88, b85, b84, b82, b80, b78, b77, b76, b75, b74, b73, b72, b70, b69, b67, b66, b64, b62, b61, b59, b58, b57, b56, b53, b52, b50, b49, b48, b47, b45, b44, b40, b35, b33, b30, b29, b28, b27, b26, b25, b24, b23, b21, b20, b19, b18, b15, b14, b13, b11, b10, b8, b7, b6, b5, b3, b2, b1, b0, c98, c96, c91, c89, c88, c87, c86, c85, c84, c83, c82, c78, c75, c74, c72, c71, c69, c68, c67, c65, c62, c60, c59, c58, c56, c55, c54, c50, c49, c48, c47, c46, c45, c44, c42, c41, c39, c37, c34, c32, c31, c27, c26, c25, c20, c18, c17, c16, c14, c13, c12, c7, c6, c4, c3, c1, c0, d96, d95, d94, d92, d91, d90, d89, d88, d87, d84, d83, d82, d80, d79, d78, d77, d76, d73, d72, d71, d69, d68, d65, d62, d60, d58, d57, d56, d55, d54, d53, d52, d49, d48, d47, d44, d43, d42, d41, d40, d38, d37, d36, d33, d32, d31, d30, d28, d27, d23, d22, d21, d20, d19, d18, d17, d16, d15, d14, d13, d12, d11, d10, d9, d8, d7, d6, d5, d1, d0, e99, e98, e95, e94, e93, e91, e90, e88, e86, e84, e83, e82, e80, e78, e75, e74, e73, e72, e70, e69, e67, e64, e63, e62, e60, e59, e58, e57, e56, e50, e47, e46, e45, e44, e43, e41, e40, e39, e36, e35, e34, e33, e30, e29, e28, e27, e26, e25, e24, e23, e22, e21, e19, e18, e15, e11, e9, e8, e7, e6, e5, e4, e3, e2, e1, f99, f98, f97, f96, f89, f88, f86, f85, f84, f83, f82, f80, f77, f76, f75, f74, f72, f71, f69, f68, f66, f64, f63, f62, f61, f60, f59, f58, f56, f55, f52, f50, f48, f45, f42, f40, f38, f35, f34, f33, f32, f31, f30, f29, f27, f24, f22, f21, f20, f19, f18, f17, f15, f10, f9, f8, f7, f6, f2, f1, f0, g98, g97, g96, g94, g93, g92, g88, g84, g80, g78, g75, g72, g70, g68, g67, g66, g65, g64, g62, g61, g58, g57, g55, g54, g52, g50, g49, g46, g41, g39, g38, g37, g36, g35, g33, g32, g30, g29, g28, g27, g25, g22, g18, g17, g16, g14, g11, g10, g9, g7, g6, g3, g1, g0, h97, h94, h92, h90, h89, h86, h84, h81, h80, h79, h78, h77, h74, h72, h69, h66, h65, h64, h63, h62, h61, h60, h59, h58, h57, h56, h54, h52, h48, h47, h46, h45, h43, h42, h41, h40, h37, h36, h34, h31, h29, h27, h26, h25, h24, h23, h20, h18, h17, h15, h13, h12, h11, h10, h8, h7, h5, h4, h3, h2, h1, i99, i98, i97, i96, i94, i93, i89, i87, i86, i84, i78, i77, i76, i75, i73, i72, i71, i70, i69, i66, i64, i63, i62, i60, i58, i57, i55, i54, i43, i42, i41, i40, i39, i32, i31, i30, i28, i27, i23, i22, i21, i20, i19, i18, i16, i15, i14, i13, i12, i10, i8, i7, i3, i2, i1, i0, j99, j98, j97, j96, j93, j92, j91, j90, j89, j87, j85, j84, j83, j81, j79, j78, j76, j75, j74, j73, j72, j71, j69, j68, j67, j65, j64, j62, j61, j60, j59, j58, j56, j54, j52, j50, j49, j48, j47, j46, j45, j44, j43, j40, j39, j35, j32, j31, j28, j27, j26, j25, j23, j22, j19, j18, j17, j15, j14, j11, j10, j8, j7, j6, j3, j1, j0, k99, k97, k96, k94, k93, k92, k90, k89, k87, k86, k84, k81, k80, k78, k73, k71, k70, k69, k68, k66, k65, k64, k62, k61, k60, k59, k58, k55, k54, k52, k51, k50, k49, k48, k47, k45, k43, k42, k41, k40, k39, k38, k37, k35, k33, k32, k30, k29, k26, k24, k23, k22, k21, k19, k18, k17, k16, k10, k9, k8, k7, k6, k5, k2, k1, k0, l98, l97, l96, l95, l94, l93, l92, l90, l89, l88, l87, l85, l83, l82, l81, l78, l77, l76, l74, l72, l70, l69, l68, l67, l66, l64, l62, l60, l59, l56, l55, l54, l53, l52, l50, l49, l47, l46, l44, l42, l40, l39, l34, l32, l31, l30, l28, l25, l24, l22, l20, l19, l17, l15, l14, l13, l12, l11, l9, l8, l6, l4, l2, l1, l0, m99, m96, m95, m94, m90, m82, m81, m80, m79, m78, m75, m73, m71, m70, m67, m66, m65, m64, m62, m61, m60, m57, m56, m55, m54, m53, m52, m51, m50, m49, m46, m44, m43, m42, m41, m40, m39, m38, m37, m34, m32, m31, m29, m26, m24, m23, m22, m20, m19, m17, m16, m15, m14, m13, m11, m6, m4, m3, m1, n98, n97, n96, n95, n92, n90, n89, n88, n87, n86, n85, n84, n83, n80, n77, n76, n75, n73, n72, n70, n68, n67, n65, n64, n62, n61, n59, n58, n57, n55, n54, n53, n52, n50, n49, n47, n46, n45, n44, n43, n42, n41, n39, n37, n34, n33, n28, n26, n25, n24, n23, n21, n20, n19, n18, n17, n15, n14, n13, n11, n10, n9, n7, n5, n1, n0, o98, o97, o94, o93, o90, o88, o85, o84, o83, o79, o76, o74, o71, o69, o68, o67, o65, o64, o62, o61, o60, o59, o55, o54, o52, o51, o50, o48, o44, o42, o41, o40, o39, o38, o37, o36, o29, o28, o25, o24, o23, o21, o20, o19, o18, o17, o16, o14, o10, o9, o8, o7, o6, o5, o4, o3, o2, o1, o0, p98, p96, p95, p94, p92, p89, p88, p87, p86, p83, p81, p80, p78, p76, p74, p71, p63, p60, p55, p54, p53, p51, p50, p48, p47, p46, p44, p38, p37, p36, p34, p32, p29, p28, p27, p26, p25, p24, p23, p21, p20, p19, p18, p14, p13, p12, p11, p10, p9, p8, p7, p6, p3, p2, p1, q99, q98, q97, q96, q93, q92, q91, q90, q88, q87, q86, q85, q83, q81, q80, q78, q72, q68, q64, q63, q62, q60, q59, q56, q55, q54, q52, q51, q49, q48, q45, q42, q39, q38, q36, q34, q33, q32, q31, q30, q29, q28, q27, q26, q24, q23, q21, q20, q18, q17, q16, q14, q12, q11, q10, q9, q8, q6, q4, q3, q2, q1, q0, r99, r97, r94, r93, r92, r91, r90, r89, r88, r84, r83, r82, r81, r80, r77, r76, r74, r73, r72, r71, r69, r68, r67, r66, r64, r63, r60, r59, r58, r56, r55, r51, r50, r49, r48, r47, r46, r45, r44, r43, r42, r40, r39, r38, r37, r36, r35, r34, r32, r30, r25, r24, r22, r21, r20, r19, r15, r13, r12, r10, r9, r8, r4, r3, r2, s99, s98, s97, s92, s91, s90, s88, s86, s83, s82, s77, s75, s74, s73, s72, s71, s68, s67, s65, s64, s63, s61, s59, s57, s56, s55, s53, s51, s49, s48, s47, s46, s42, s41, s40, s39, s37, s36, s35, s34, s33, s30, s28, s27, s26, s25, s24, s23, s22, s15, s14, s13, s11, s9, s8, s7, s4, s2, s0, t99, t98, t96, t95, t92, t90, t89, t88, t86, t85, t84, t83, t81, t80, t79, t77, t73, t71, t70, t69, t67, t65, t62, t58, t57, t55, t53, t52, t50, t49, t48, t47, t45, t42, t41, t40, t39, t38, t37, t36, t35, t34, t33, t30, t29, t26, t25, t23, t22, t21, t20, t16, t11, t9, t3, t0, u94, u91, u89, u88, u87, u86, u84, u83, u82, u81, u80, u79, u78, u77, u76, u74, u73, u72, u71, u68, u66, u65, u64, u62, u61, u60, u59, u58, u57, u56, u53, u52, u51, u50, u48, u47, u45, u44, u43, u41, u40, u39, u37, u36, u35, u32, u30, u29, u28, u27, u24, u22, u21, u17, u15, u11, u9, u8, u7, u5, u3, v99, v97, v95, v94, v93, v92, v89, v88, v86, v85, v84, v81, v80, v79, v78, v76, v75, v74, v73, v72, v71, v69, v68, v66, v65, v64, v62, v59, v58, v56, v55, v54, v52, v47, v45, v43, v40, v37, v35, v34, v33, v32, v31, v29, v28, v27, v26, v25, v20, v19, v18, v17, v14, v13, v11, v7, v5, v4, v3, v2, w96, w95, w92, w91, w87, w86, w84, w83, w81, w79, w75, w71, w70, w69, w68, w66, w64, w63, w62, w59, w57, w55, w54, w50, w48, w47, w44, w43, w42, w41, w40, w39, w37, w36, w35, w34, w32, w31, w30, w29, w28, w27, w25, w23, w22, w19, w18, w16, w14, w12, w9, w7, w5, w3, w2, w1, w0, x99, x98, x96, x95, x94, x88, x85, x84, x83, x80, x77, x76, x75, x74, x69, x68, x65, x64, x63, x62, x61, x60, x56, x54, x53, x52, x49, x46, x45, x44, x43, x42, x40, x37, x36, x35, x34, x33, x32, x31, x29, x28, x27, x26, x25, x24, x23, x21, x20, x18, x17, x16, x15, x14, x13, x10, x7, x5, x4, x2, y99, y98, y97, y91, y90, y89, y88, y87, y86, y84, y79, y78, y77, y76, y75, y73, y72, y71, y69, y68, y67, y66, y65, y64, y63, y62, y59, y56, y55, y52, y51, y50, y49, y48, y47, y46, y45, y43, y42, y41, y39, y35, y33, y32, y31, y29, y28, y27, y26, y25, y21, y18, y16, y15, y14, y13, y12, y10, y9, y8, y7, y6, y3, y1, z98, z96, z95, z94, z93, z92, z90, z89, z87, z85, z84, z83, z82, z80, z79, z77, z76, z75, z73, z72, z69, z68, z66, z65, z64, z61, z59, z58, z57, z54, z53, z52, z50, z47, z46, z44, z41, z39, z38, z37, z36, z35, z33, z27, z26, z25, z24, z22, z21, z19, z18, z17, z15, z13, z12, z10, z9, z8, z6, z5, z4, z3, z2, z1, z0
```

<div style="page-break-after: always; break-after: page;"></div>



## Week-2 Problem 2

Merging two sorted arrays in place.

Given a custom implementation of list named `MyList`. On `MyList` objects you can perform read operations similar to the in-build lists in Python, example use `A[i]` to read element at index `i` in `MyList`object `A`. The only possible operation that you can use to edit data in `MyList` objects is by calling the `swap` method. For instance, `A.swap(indexA, B, indexB)` will swap values at `A[indexA]` and `B[indexB]`and `A.swap(index1, A, index2)` will swap values at `A[index1]` and `A[index2]`, where `indexA`,  `indexB`,  `index1`, `index2` are all integers.

Complete the Python function `mergeInPlace(A, B)` that accepts two `MyLists` `A` and `B` containing integers that are sorted in ascending order and merges them in place(without using any other list) such that after merging, `A` and `B` are still sorted in ascending order with the smallest element of both `MyLists` as the first element of `A`. 



```python
# Complete this function
def mergeInPlace(A, B):
  # Your code goes here
```

***Sample Input:***

```
2 4 6 9 13 15
1 3 5 10
```

***Sample Output:***

```
1 2 3 4 5 6
9 10 13 15
```

***Sample Input:***

```
4 6 
1 3 6 10
```

***Sample Output:***

```
1 3
4 6 6 10
```



### Solution

```python
def mergeInPlace(A, B):
  m = len(A)
  n = len(B)
  if (m < 1 or n < 1):
    return 
  
  # Find the smaller list of A and B.
  for i in range(0, m):
    # A and B are already sorted. B[0] will always be least in B, 
    # as we will maintain its sortedness .
    if (A[i] > B[0]):
      A.swap(i, B, 0)
       
      # move `B[0]` to its correct position in B to maintain the sortedness of B
      j = 0
      while(j < n - 1 and B[j] > B[j + 1]):
        B.swap(j+1, B, j)        
        j += 1

```

**Suffix(Hidden)**

```python
class List:
  def __init__(self):
    self.__L = [int(i) for i in input().strip().split()]

  def __len__(self):
    return len(self.__L)

  def swap(self, i, obj, j):
    if isinstance(obj, List) and isinstance(i, int) and isinstance(j, int):
      self.__L[i], obj.__L[j] = obj.__L[j], self.__L[i]
    else:
      message = 'Invalid argument type. Swap can be called on either object A or object B, and accepts three arguments of type (<int>, <MyList Object>, <int>) in the same sequence'
      raise Exception(message)

  def __getitem__(self, x):
    return self.__L[x]

  def __str__(self):
    return str(self.__L)

A = List()
B = List()

mergeInPlace(A, B)
print(*A)
print(*B)
```

**Public Test Case 1**

**Input**

```
2 4 6 9 13 15
1 3 5 10
```

**Output**

```
1 2 3 4 5 6
9 10 13 15
```

**Public Test Case 2**

**Input**

```
4 6 
1 3 6 10
```

**Output**

```
1 3
4 6 6 10
```

**Private Test Case 1**

**Input**

```
4 6 
1 3 6 10
```

**Output**

```
1 3
4 6 6 10
```

**Private Test Case 2**

**Input**

```
25 27 75 77 157 210 221 234 240 241 316 396 435 467 557 643 643 716 720 798 824 893 929 979 1054 1067 1260 1322 1391 1393 1540 1580 1770 1794 1824 1835 1915 1983 1996 2034 2036 2086 2184 2311 2327 2354 2367 2372 2390 2401 2489 2490 2500 2523 2528 2582 2697 2732 2763 2779 2838 2976 2985 2990 3007 3098 3115 3163 3191 3269 3306 3439 3502 3505 3536 3573 3590 3645 3702 3705 3861 3913 4144 4238 4244 4363 4407 4667 4689 4767 4854 4894 4985 5000 5001 5068 5119 5309 5339 5398 5496 5533 5562 5575 5589 5661 5704 5786 5829 5924 5957 6015 6103 6133 6214 6226 6244 6258 6276 6306 6318 6327 6363 6388 6406 6436 6454 6572 6735 6881 6952 6966 6971 7041 7070 7093 7248 7275 7283 7307 7308 7330 7352 7365 7390 7436 7516 7531 7585 7600 7626 7660 7731 7742 7758 7791 7848 7871 7872 7898 7932 7972 8017 8033 8089 8127 8280 8324 8390 8425 8471 8485 8553 8636 8713 8781 8931 8990 8999 9024 9087 9107 9107 9188 9213 9218 9228 9285 9372 9456 9556 9568 9574 9574 9678 9816 9848 9954 9968 9985
45 53 71 79 98 112 119 131 164 173 184 210 245 286 359 391 398 418 446 449 460 462 467 472 499 509 511 525 527 547 570 575 579 634 641 655 661 679 692 715 719 757 771 806 808 854 873 919 922 961 1051 1052 1060 1121 1165 1187 1221 1245 1251 1252 1287 1343 1362 1363 1384 1460 1471 1533 1555 1587 1594 1608 1612 1638 1730 1730 1766 1766 1767 1774 1843 1865 1872 1874 1925 1982 2010 2113 2114 2121 2132 2148 2175 2191 2228 2274 2279 2280 2326 2337 2443 2455 2464 2467 2494 2502 2567 2570 2584 2586 2587 2595 2597 2617 2659 2689 2712 2722 2724 2725 2733 2751 2752 2757 2813 2878 2914 2940 2943 2946 2955 2965 2987 2998 3050 3068 3108 3129 3154 3171 3225 3274 3281 3298 3309 3318 3329 3335 3351 3354 3357 3405 3413 3440 3486 3490 3495 3505 3518 3521 3541 3543 3548 3560 3588 3605 3612 3614 3655 3683 3689 3709 3734 3745 3770 3778 3783 3787 3788 3811 3825 3828 3838 3850 3858 3863 3871 3884 3887 3896 3907 3920 3969 3995 4030 4037 4077 4089 4115 4139 4159 4169 4178 4204 4220 4253 4258 4267 4279 4280 4290 4343 4373 4385 4403 4414 4433 4446 4466 4480 4493 4521 4533 4538 4563 4588 4622 4641 4645 4656 4715 4726 4749 4783 4797 4799 4866 4877 4881 4911 4916 4961 4970 4990 5005 5019 5030 5039 5045 5054 5076 5077 5093 5102 5119 5131 5135 5154 5166 5178 5179 5204 5252 5259 5276 5316 5326 5345 5374 5375 5397 5406 5410 5416 5418 5433 5442 5492 5500 5531 5565 5568 5571 5714 5719 5757 5768 5813 5822 5832 5839 5865 5877 5905 5925 5926 5958 5967 5968 5988 6008 6027 6029 6033 6055 6077 6086 6087 6088 6111 6128 6133 6146 6159 6199 6218 6237 6260 6316 6354 6377 6420 6420 6457 6468 6473 6550 6553 6564 6570 6573 6590 6645 6645 6682 6694 6697 6698 6728 6747 6751 6760 6762 6765 6778 6779 6788 6794 6818 6821 6824 6844 6844 6892 6906 6938 6968 7029 7040 7041 7063 7095 7101 7102 7119 7142 7200 7222 7225 7231 7233 7243 7253 7254 7297 7303 7315 7317 7328 7365 7372 7389 7391 7410 7439 7445 7458 7464 7476 7540 7571 7585 7628 7628 7631 7642 7643 7711 7728 7734 7749 7750 7755 7759 7768 7826 7836 7881 7902 7950 7997 8000 8004 8026 8037 8042 8077 8096 8116 8127 8135 8148 8170 8189 8191 8223 8223 8236 8258 8278 8285 8296 8315 8319 8322 8331 8358 8373 8404 8423 8435 8466 8489 8490 8494 8531 8543 8548 8585 8613 8639 8698 8703 8706 8708 8735 8740 8743 8754 8756
```

**Output**

```
25 27 45 53 71 75 77 79 98 112 119 131 157 164 173 184 210 210 221 234 240 241 245 286 316 359 391 396 398 418 435 446 449 460 462 467 467 472 499 509 511 525 527 547 557 570 575 579 634 641 643 643 655 661 679 692 715 716 719 720 757 771 798 806 808 824 854 873 893 919 922 929 961 979 1051 1052 1054 1060 1067 1121 1165 1187 1221 1245 1251 1252 1260 1287 1322 1343 1362 1363 1384 1391 1393 1460 1471 1533 1540 1555 1580 1587 1594 1608 1612 1638 1730 1730 1766 1766 1767 1770 1774 1794 1824 1835 1843 1865 1872 1874 1915 1925 1982 1983 1996 2010 2034 2036 2086 2113 2114 2121 2132 2148 2175 2184 2191 2228 2274 2279 2280 2311 2326 2327 2337 2354 2367 2372 2390 2401 2443 2455 2464 2467 2489 2490 2494 2500 2502 2523 2528 2567 2570 2582 2584 2586 2587 2595 2597 2617 2659 2689 2697 2712 2722 2724 2725 2732 2733 2751 2752 2757 2763 2779 2813 2838 2878 2914 2940 2943 2946 2955 2965 2976 2985 2987 2990 2998 3007 3050
3068 3098 3108 3115 3129 3154 3163 3171 3191 3225 3269 3274 3281 3298 3306 3309 3318 3329 3335 3351 3354 3357 3405 3413 3439 3440 3486 3490 3495 3502 3505 3505 3518 3521 3536 3541 3543 3548 3560 3573 3588 3590 3605 3612 3614 3645 3655 3683 3689 3702 3705 3709 3734 3745 3770 3778 3783 3787 3788 3811 3825 3828 3838 3850 3858 3861 3863 3871 3884 3887 3896 3907 3913 3920 3969 3995 4030 4037 4077 4089 4115 4139 4144 4159 4169 4178 4204 4220 4238 4244 4253 4258 4267 4279 4280 4290 4343 4363 4373 4385 4403 4407 4414 4433 4446 4466 4480 4493 4521 4533 4538 4563 4588 4622 4641 4645 4656 4667 4689 4715 4726 4749 4767 4783 4797 4799 4854 4866 4877 4881 4894 4911 4916 4961 4970 4985 4990 5000 5001 5005 5019 5030 5039 5045 5054 5068 5076 5077 5093 5102 5119 5119 5131 5135 5154 5166 5178 5179 5204 5252 5259 5276 5309 5316 5326 5339 5345 5374 5375 5397 5398 5406 5410 5416 5418 5433 5442 5492 5496 5500 5531 5533 5562 5565 5568 5571 5575 5589 5661 5704 5714 5719 5757 5768 5786 5813 5822 5829 5832 5839 5865 5877 5905 5924 5925 5926 5957 5958 5967 5968 5988 6008 6015 6027 6029 6033 6055 6077 6086 6087 6088 6103 6111 6128 6133 6133 6146 6159 6199 6214 6218 6226 6237 6244 6258 6260 6276 6306 6316 6318 6327 6354 6363 6377 6388 6406 6420 6420 6436 6454 6457 6468 6473 6550 6553 6564 6570 6572 6573 6590 6645 6645 6682 6694 6697 6698 6728 6735 6747 6751 6760 6762 6765 6778 6779 6788 6794 6818 6821 6824 6844 6844 6881 6892 6906 6938 6952 6966 6968 6971 7029 7040 7041 7041 7063 7070 7093 7095 7101 7102 7119 7142 7200 7222 7225 7231 7233 7243 7248 7253 7254 7275 7283 7297 7303 7307 7308 7315 7317 7328 7330 7352 7365 7365 7372 7389 7390 7391 7410 7436 7439 7445 7458 7464 7476 7516 7531 7540 7571 7585 7585 7600 7626 7628 7628 7631 7642 7643 7660 7711 7728 7731 7734 7742 7749 7750 7755 7758 7759 7768 7791 7826 7836 7848 7871 7872 7881 7898 7902 7932 7950 7972 7997 8000 8004 8017 8026 8033 8037 8042 8077 8089 8096 8116 8127 8127 8135 8148 8170 8189 8191 8223 8223 8236 8258 8278 8280 8285 8296 8315 8319 8322 8324 8331 8358 8373 8390 8404 8423 8425 8435 8466 8471 8485 8489 8490 8494 8531 8543 8548 8553 8585 8613 8636 8639 8698 8703 8706 8708 8713 8735 8740 8743 8754 8756 8781 8931 8990 8999 9024 9087 9107 9107 9188 9213 9218 9228 9285 9372 9456 9556 9568 9574 9574 9678 9816 9848 9954 9968 9985
```

**Private Test Case 3**

**Input**

```
31 36 53 69 107 182 197 198 201 207 224 262 301 364 384 389 552 585 588 608 635 640 648 648 753 761 762 765 801 875 926 928 929 980 982 1057 1079 1110 1111 1112 1133 1171 1179 1211 1217 1218 1227 1348 1366 1412 1422 1427 1430 1475 1489 1490 1493 1529 1547 1558 1575 1582 1603 1614 1651 1654 1669 1699 1709 1717 1729 1740 1786 1791 1799 1842 1844 1874 1913 1931 1940 1941 1971 1998 2011 2031 2081 2097 2105 2106 2134 2140 2144 2151 2175 2211 2229 2230 2238 2241 2254 2291 2298 2309 2355 2394 2406 2424 2522 2553 2606 2648 2651 2739 2758 2761 2787 2809 2855 2883 2887 2893 2895 2906 2914 2919 2920 2940 3002 3025 3034 3036 3039 3039 3077 3077 3114 3146 3147 3153 3170 3171 3194 3223 3236 3247 3277 3313 3317 3324 3328 3336 3362 3369 3370 3377 3382 3392 3395 3399 3423 3450 3453 3484 3506 3514 3524 3534 3542 3546 3555 3560 3573 3606 3626 3639 3672 3705 3709 3730 3784 3786 3789 3841 3869 3872 3937 3941 3968 3974 3988 3998 4015 4028 4044 4050 4076 4089 4109 4166 4192 4193 4273 4274 4291 4365 4411 4412 4414 4440 4440 4452 4464 4487 4492 4492 4545 4565 4595 4603 4612 4619 4623 4624 4633 4681 4686 4720 4722 4723 4742 4747 4762 4824 4837 4849 4875 4889 4923 4932 4933 4951 4960 4967 4986 5015 5021 5062 5077 5087 5090 5114 5115 5133 5134 5137 5143 5149 5159 5163 5176 5192 5205 5257 5263 5263 5267 5267 5270 5289 5309 5316 5320 5325 5336 5370 5381 5386 5393 5427 5429 5430 5462 5512 5543 5545 5553 5555 5564 5571 5580 5584 5629 5639 5641 5678 5681 5685 5688 5699 5719 5728 5752 5795 5805 5808 5809 5822 5863 5902 5942 5986 5986 6024 6047 6064 6072 6092 6114 6134 6171 6196 6275 6281 6323 6325 6327 6339 6347 6393 6430 6460 6480 6483 6518 6540 6552 6558 6559 6586 6594 6600 6608 6623 6629 6654 6688 6714 6734 6749 6798 6827 6830 6846 6938 6942 7003 7022 7047 7057 7073 7078 7086 7102 7127 7131 7149 7159 7181 7208 7216 7227 7296 7307 7328 7337 7412 7429 7445 7448 7453 7493 7498 7502 7522 7580 7600 7610 7611 7614 7616 7623 7629 7683 7701 7745 7783 7796 7846 7873 7876 7877 7945 7957 7969 7974 7987 8002 8026 8032 8044 8062 8084 8120 8126 8157 8171 8236 8253 8278 8283 8292 8303 8364 8381 8390 8391 8401 8402 8423 8430 8510 8553 8556 8565 8573 8586 8595 8690 8707 8709 8767 8769 8778 8786 8801 8838 8846 8848 8863 8865 8918 8941 8960 8979 9058 9062 9089 9187 9235 9304 9307 9319 9330 9339 9361 9365 9369 9387 9406 9461 9519 9526 9531 9547 9554 9566 9567 9575 9612 9638 9642 9645 9668 9669 9723 9766 9780 9793 9817 9862 9866 9895 9902 9907 9916 9963 9981 9993 9996
54 268 644 1263 1297 1341 1524 1747 1771 1836 2033 2071 2503 2637 2923 3059 3325 3957 4106 4163 4200 4363 4643 5075 5307 5511 5530 5556 5572 5938 6620 6942 7018 7146 7442 7465 7605 7673 8024 8181 8669 8852 9394 9441 9625 9739
```

**Output**

```
31 36 53 54 69 107 182 197 198 201 207 224 262 268 301 364 384 389 552 585 588 608 635 640 644 648 648 753 761 762 765 801 875 926 928 929 980 982 1057 1079 1110 1111 1112 1133 1171 1179 1211 1217 1218 1227 1263 1297 1341 1348 1366 1412 1422 1427 1430 1475 1489 1490 1493 1524 1529 1547 1558 1575 1582 1603 1614 1651 1654 1669 1699 1709 1717 1729 1740 1747 1771 1786 1791 1799 1836 1842 1844 1874 1913 1931 1940 1941 1971 1998 2011 2031 2033 2071 2081 2097 2105 2106 2134 2140 2144 2151 2175 2211 2229 2230 2238 2241 2254 2291 2298 2309 2355 2394 2406 2424 2503 2522 2553 2606 2637 2648 2651 2739 2758 2761 2787 2809 2855 2883 2887 2893 2895 2906 2914 2919 2920 2923 2940 3002 3025 3034 3036 3039 3039 3059 3077 3077 3114 3146 3147 3153 3170 3171 3194 3223 3236 3247 3277 3313 3317 3324 3325 3328 3336 3362 3369 3370 3377 3382 3392 3395 3399 3423 3450 3453 3484 3506 3514 3524 3534 3542 3546 3555 3560 3573 3606 3626 3639 3672 3705 3709 3730 3784 3786 3789 3841 3869 3872 3937 3941 3957 3968 3974 3988 3998 4015 4028 4044 4050 4076 4089 4106 4109 4163 4166 4192 4193 4200 4273 4274 4291 4363 4365 4411 4412 4414 4440 4440 4452 4464 4487 4492 4492 4545 4565 4595 4603 4612 4619 4623 4624 4633 4643 4681 4686 4720 4722 4723 4742 4747 4762 4824 4837 4849 4875 4889 4923 4932 4933 4951 4960 4967 4986 5015 5021 5062 5075 5077 5087 5090 5114 5115 5133 5134 5137 5143 5149 5159 5163 5176 5192 5205 5257 5263 5263 5267 5267 5270 5289 5307 5309 5316 5320 5325 5336 5370 5381 5386 5393 5427 5429 5430 5462 5511 5512 5530 5543 5545 5553 5555 5556 5564 5571 5572 5580 5584 5629 5639 5641 5678 5681 5685 5688 5699 5719 5728 5752 5795 5805 5808 5809 5822 5863 5902 5938 5942 5986 5986 6024 6047 6064 6072 6092 6114 6134 6171 6196 6275 6281 6323 6325 6327 6339 6347 6393 6430 6460 6480 6483 6518 6540 6552 6558 6559 6586 6594 6600 6608 6620 6623 6629 6654 6688 6714 6734 6749 6798 6827 6830 6846 6938 6942 6942 7003 7018 7022 7047 7057 7073 7078 7086 7102 7127 7131 7146 7149 7159 7181 7208 7216 7227 7296 7307 7328 7337 7412 7429 7442 7445 7448 7453 7465 7493 7498 7502 7522 7580 7600 7605 7610 7611 7614 7616 7623 7629 7673 7683 7701 7745 7783 7796 7846 7873 7876 7877 7945 7957 7969 7974 7987 8002 8024 8026 8032 8044 8062 8084 8120 8126 8157 8171 8181 8236 8253 8278 8283 8292 8303 8364 8381 8390 8391 8401 8402 8423 8430 8510 8553 8556 8565 8573 8586 8595 8669 8690 8707 8709 8767 8769 8778 8786 8801 8838 8846 8848 8852 8863 8865 8918 8941 8960 8979 9058 9062 9089
9187 9235 9304 9307 9319 9330 9339 9361 9365 9369 9387 9394 9406 9441 9461 9519 9526 9531 9547 9554 9566 9567 9575 9612 9625 9638 9642 9645 9668 9669 9723 9739 9766 9780 9793 9817 9862 9866 9895 9902 9907 9916 9963 9981 9993 9996
```

**Private Test Case 4**

**Input**

```
21 24 26 30 37 37 55 76 78 86 98 128 171 233 285 289 309 309 311 319 326 335 338 360 380 401 425 442 442 480 487 490 492 509 534 544 568 624 626 636 700 712 732 753 758 769 772 777 784 785 807 835 859 862 874 875 881 918 948 953 981 1044 1061 1073 1103 1121 1122 1138 1162 1171 1184 1189 1189 1207 1209 1228 1244 1246 1276 1282 1285 1304 1321 1366 1382 1434 1442 1444 1450 1451 1462 1489 1521 1521 1531 1532 1544 1550 1558 1583 1585 1590 1596 1620 1624 1634 1654 1666 1745 1754 1755 1775 1793 1804 1821 1830 1831 1863 1873 1877 1878 1928 1929 1948 1949 1979 2008 2053 2058 2120 2120 2129 2183 2185 2197 2203 2204 2219 2225 2225 2278 2291 2352 2366 2367 2387 2403 2417 2426 2446 2447 2456 2469 2537 2539 2564 2568 2569 2578 2593 2605 2612 2614 2630 2670 2705 2725 2727 2815 2836 2848 2894 2921 2980 2997 3025 3030 3071 3079 3082 3100 3134 3161 3228 3235 3256 3307 3337 3348 3364 3413 3468 3470 3475 3477 3483 3495 3496 3507 3518 3519 3571 3588 3589 3607 3625 3646 3665 3735 3743 3755 3817 3836 3855 3866 3881 3904 3907 3939 3947 3950 3980 4054 4068 4111 4112 4117 4147 4181 4182 4203 4218 4247 4259 4266 4278 4299 4303 4328 4337 4370 4383 4402 4414 4420 4433 4437 4450 4457 4466 4476 4484 4527 4594 4648 4658 4672 4675 4701 4708 4723 4729 4758 4775 4782 4784 4795 4795 4799 4825 4830 4857 4864 4865 4931 4937 4949 4962 5015 5044 5075 5097 5131 5156 5185 5190 5201 5219 5233 5243 5251 5265 5273 5337 5375 5375 5383 5451 5452 5460 5507 5545 5562 5602 5617 5689 5705 5729 5739 5748 5752 5765 5782 5789 5790 5795 5803 5849 5852 5858 5867 5912 5922 5923 5941 5942 5947 5972 5975 6005 6008 6040 6044 6047 6065 6074 6093 6114 6118 6143 6151 6184 6193 6194 6215 6221 6236 6244 6251 6252 6254 6275 6281 6295 6383 6388 6394 6455 6496 6517 6519 6525 6568 6623 6633 6645 6650 6655 6664 6707 6713 6746 6777 6809 6819 6831 6842 6844 6853 6862 6879 6885 6893 6894 6901 6911 6922 6935 6967 7019 7044 7058 7062 7111 7120 7135 7153 7158 7184 7220 7237 7278 7301 7312 7338 7342 7352 7369 7370 7373 7388 7393 7395 7424 7440 7493 7505 7523 7574 7575 7609 7611 7647 7655 7662 7665 7667 7672 7681 7683 7701 7734 7762 7763 7769 7772 7781 7820 7822 7836 7884 7900 7902 7954 7960 7969 7971 7986 8037 8056 8124 8128 8133 8137 8153 8207 8208 8223 8225 8235 8245 8283 8289 8300 8300 8301 8309 8323 8326 8365 8370 8422 8425 8429 8440 8454 8472 8476 8491 8500 8514 8562 8590 8594 8598 8602 8607 8616 8624 8644 8649 8657 8690 8702 8708 8723 8756 8779 8783 8814 8836 8859 8930 8934 8943 8954 8968 8990 9000 9107 9120 9137 9204 9232 9233 9251 9293 9320 9326 9329 9330 9372 9381 9388 9390 9414 9418 9420 9429 9431 9441 9450 9461 9463 9464 9465 9478 9536 9565 9595 9619 9625 9654 9694 9702 9709 9726 9732 9743 9753 9760 9778 9841 9888 9912 9917 9919 9937 9979 9985
17 21 29 47 48 66 80 86 100 102 104 107 125 127 133 143 147 153 158 167 169 186 188 199 203 231 239 287 291 334 341 359 369 373 376 378 392 393 397 398 416 422 426 430 434 435 448 456 463 480 483 488 498 501 505 520 538 546 548 571 593 593 594 615 629 636 658 661 665 690 700 708 728 731 742 759 761 774 785 801 849 850 865 869 872 874 876 876 882 897 900 905 927 935 955 978 981 992 1001 1015 1041 1047 1048 1050 1056 1072 1078 1083 1085 1111 1113 1115 1132 1139 1169 1170 1177 1179 1182 1200 1215 1238 1246 1274 1287 1290 1297 1298 1300 1322 1325 1379 1386 1415 1428 1469 1485 1485 1501 1505 1521 1554 1562 1567 1572 1577 1579 1587 1589 1606 1615 1621 1622 1631 1637 1648 1660 1670 1689 1699 1699 1713 1740 1759 1765 1789 1794 1798 1798 1811 1825 1828 1854 1868 1879 1904 1911 1915 1920 1938 1941 1954 1968 1999 2007 2009 2034 2036 2056 2071 2085 2086 2106 2107 2112 2127 2131 2137 2139 2140 2156 2164 2165 2171 2173 2189 2212 2231 2232 2241 2243 2244 2260 2265 2284 2304 2305 2320 2328 2345 2347 2351 2371 2405 2421 2433 2449 2469 2475 2482 2491 2491 2502 2509 2524 2540 2561 2570 2599 2602 2608 2644 2645 2652 2658 2690 2704 2721 2736 2762 2772 2772 2774 2775 2780 2784 2823 2831 2839 2864 2871 2872 2889 2890 2909 2911 2911 2925 2925 2932 2936 2950 2984 3006 3010 3018 3027 3029 3041 3043 3049 3080 3090 3112 3114 3117 3121 3171 3181 3194 3212 3216 3218 3222 3226 3244 3249 3262 3286 3307 3314 3315 3322 3340 3347 3352 3352 3352 3367 3382 3387 3390 3390 3407 3410 3416 3423 3432 3452 3465 3471 3476 3478 3483 3507 3511 3523 3533 3544 3576 3587 3594 3596 3601 3606 3607 3613 3616 3629 3632 3652 3656 3699 3717 3723 3725 3736 3736 3738 3753 3754 3761 3780 3796 3797 3816 3830 3831 3833 3839 3853 3892 3903 3905 3916 3916 3919 3926 3943 3954 3959 3960 3988 3988 3989 4049 4063 4066 4068 4083 4092 4094 4105 4114 4117 4147 4157 4162 4163 4171 4188 4197 4199 4230 4242 4249 4250 4251 4285 4307 4310 4327 4328 4337 4347 4356 4376 4392 4393 4399 4402 4411 4411 4417 4422 4423 4425 4431 4436 4467 4484 4506 4506 4514 4521 4523 4523 4537 4560 4576 4579 4579 4580 4585 4593 4635 4640 4659 4662 4663 4675 4676 4684 4691 4703 4729 4731 4771 4788 4827 4838 4842 4842 4849 4852 4859 4868 4870 4885 4889 4889 4903 4914 4967 4981 4984 4987 4987 4989 4997 5004 5012 5031 5032 5063 5068 5073 5080 5092 5123 5130 5144 5147 5157 5159 5172 5217 5222 5228 5233 5239 5241 5245 5250 5275 5277 5303 5305 5306 5316 5317 5322 5332 5337 5354 5365 5372 5374 5388 5399 5406 5414 5417 5442 5452 5462 5493 5500 5526 5533 5535 5555 5558 5561 5561 5562 5562 5577 5579 5580 5606 5623 5625 5643 5645 5650 5655 5662 5664 5664 5680 5683 5691 5702 5718 5739 5743 5756 5763 5792 5794 5798 5803 5813 5815 5829 5833 5834 5838 5854 5861 5873 5879 5880 5885 5887 5892 5900 5910 5956 5968 6001 6041 6042 6046 6061 6063 6063 6068 6070 6074 6080 6086 6093 6106 6111 6118 6121 6128 6135 6163 6174 6198 6202 6203 6212 6221 6227 6239 6243 6264 6264 6270 6274 6285 6290 6305 6317 6324 6331 6336 6337 6348 6357 6365 6366 6371 6376 6381 6395 6407 6413 6418 6420 6442 6449 6450 6454 6465 6470 6516 6528 6528 6539 6541 6550 6551 6560 6582 6584 6613 6664 6686 6689 6701 6708 6711 6717 6724 6739 6776 6791 6792 6802 6819 6846 6867 6868 6896 6899 6901 6902 6912 6923 6926 6930 6931 6941 6944 6951 6957 6961 6988 6996 7009 7049 7066 7069 7069 7098 7107 7116 7124 7131 7133 7153 7173 7177 7192 7211 7238 7261 7261 7272 7304 7306 7306 7328 7333 7376 7389 7393 7394 7397 7426 7437 7447 7461 7484 7486 7495 7507 7508 7512 7526 7532 7538 7572 7572 7573 7594 7616 7616 7618 7620 7624 7628 7631 7664 7691 7692 7717 7723 7744 7759 7783 7787 7793 7796 7798 7819 7820 7825 7888 7929 7938 7949 7950 7972 7994 8012 8016 8023 8036 8042 8054 8058 8081 8084 8119 8133 8149 8153 8169 8176 8184 8187 8188 8190 8197 8202 8227 8239 8254 8254 8257 8266 8273 8294 8310 8333 8340 8351 8363 8368 8369 8370 8378 8379 8383 8403 8410 8412 8430 8439 8442 8443 8460 8483 8488 8489 8494 8500 8509 8546 8547 8578 8594 8595 8601 8603 8604 8642 8681 8703 8709 8710 8720 8765 8774 8785 8818 8822 8826 8838 8845 8849 8859 8872 8878 8882 8885 8912 8924 8949 8961 8964 9015 9025 9038 9045 9045 9049 9050 9058 9063 9076 9076 9080 9109 9111 9124 9136 9147 9150 9151 9159 9176 9187 9190 9193 9255 9260 9278 9281 9296 9314 9316 9329 9331 9340 9341 9343 9346 9346 9347 9347 9349 9350 9371 9375 9385 9419 9422 9435 9442 9463 9482 9486 9511 9525 9528 9533 9536 9540 9548 9553 9576 9591 9631 9650 9659 9661 9664 9671 9675 9676 9690 9700 9711 9726 9737 9742 9742 9743 9749 9775 9788 9795 9818 9831 9841 9848 9854 9855 9856 9866 9868 9870 9878 9880 9881 9887 9898 9898 9927 9930 9942 9960 9972 9980 9983 9985 9988 9990 9993
```

**Output**

```
17 21 21 24 26 29 30 37 37 47 48 55 66 76 78 80 86 86 98 100 102 104 107 125 127 128 133 143 147 153 158 167 169 171 186 188 199 203 231 233 239 285 287 289 291 309 309 311 319 326 334 335 338 341 359 360 369 373 376 378 380 392 393 397 398 401 416 422 425 426 430 434 435 442 442 448 456 463 480 480 483 487 488 490 492 498 501 505 509 520 534 538 544 546 548 568 571 593 593 594 615 624 626 629 636 636 658 661 665 690 700 700 708 712 728 731 732 742 753 758 759 761 769 772 774 777 784 785 785 801 807 835 849 850 859 862 865 869 872 874 874 875 876 876 881 882 897 900 905 918 927 935 948 953 955 978 981 981 992 1001 1015 1041 1044 1047 1048 1050 1056 1061 1072 1073 1078 1083 1085 1103 1111 1113 1115 1121 1122 1132 1138 1139 1162 1169 1170 1171 1177 1179 1182 1184 1189 1189 1200 1207 1209 1215 1228 1238 1244 1246 1246 1274 1276 1282 1285 1287 1290 1297 1298 1300 1304 1321 1322 1325 1366 1379 1382 1386 1415 1428 1434 1442 1444 1450 1451 1462 1469 1485 1485 1489 1501 1505 1521 1521 1521 1531 1532 1544 1550 1554 1558 1562 1567 1572 1577 1579 1583 1585 1587 1589 1590 1596 1606 1615 1620 1621 1622 1624 1631 1634 1637 1648 1654 1660 1666 1670 1689 1699 1699 1713 1740 1745 1754 1755 1759 1765 1775 1789 1793 1794 1798 1798 1804 1811 1821 1825 1828 1830 1831 1854 1863 1868 1873 1877 1878 1879 1904 1911 1915 1920 1928 1929 1938 1941 1948 1949 1954 1968 1979 1999 2007 2008 2009 2034 2036 2053 2056 2058 2071 2085 2086 2106 2107 2112 2120 2120 2127 2129 2131 2137 2139 2140 2156 2164 2165 2171 2173 2183 2185 2189 2197 2203 2204 2212 2219 2225 2225 2231 2232 2241 2243 2244 2260 2265 2278 2284 2291 2304 2305 2320 2328 2345 2347 2351 2352 2366 2367 2371 2387 2403 2405 2417 2421 2426 2433 2446 2447 2449 2456 2469 2469 2475 2482 2491 2491 2502 2509 2524 2537 2539 2540 2561 2564 2568 2569 2570 2578 2593 2599 2602 2605 2608 2612 2614 2630 2644 2645 2652 2658 2670 2690 2704 2705 2721 2725 2727 2736 2762 2772 2772 2774 2775 2780 2784 2815 2823 2831 2836 2839 2848 2864 2871 2872 2889 2890 2894 2909 2911 2911 2921 2925 2925 2932 2936 2950 2980 2984 2997 3006 3010 3018 3025 3027 3029 3030 3041 3043 3049 3071 3079 3080 3082 3090 3100 3112 3114 3117 3121 3134 3161 3171 3181 3194 3212 3216 3218 3222 3226 3228 3235 3244 3249 3256 3262 3286 3307 3307 3314 3315 3322 3337 3340 3347 3348 3352 3352 3352 3364 3367 3382 3387 3390 3390 3407 3410 3413 3416 3423 3432 3452 3465 3468 3470 3471 3475 3476 3477 3478 3483 3483 3495 3496 3507 3507 3511 3518 3519 3523 3533 3544 3571 3576 3587 3588 3589 3594 3596 3601 3606 3607 3607 3613 3616 3625 3629 3632 3646 3652 3656 3665 3699 3717 3723 3725 3735 3736 3736 3738 3743 3753
3754 3755 3761 3780 3796 3797 3816 3817 3830 3831 3833 3836 3839 3853 3855 3866 3881 3892 3903 3904 3905 3907 3916 3916 3919 3926 3939 3943 3947 3950 3954 3959 3960 3980 3988 3988 3989 4049 4054 4063 4066 4068 4068 4083 4092 4094 4105 4111 4112 4114 4117 4117 4147 4147 4157 4162 4163 4171 4181 4182 4188 4197 4199 4203 4218 4230 4242 4247 4249 4250 4251 4259 4266 4278 4285 4299 4303 4307 4310 4327 4328 4328 4337 4337 4347 4356 4370 4376 4383 4392 4393 4399 4402 4402 4411 4411 4414 4417 4420 4422 4423 4425 4431 4433 4436 4437 4450 4457 4466 4467 4476 4484 4484 4506 4506 4514 4521 4523 4523 4527 4537 4560 4576 4579 4579 4580 4585 4593 4594 4635 4640 4648 4658 4659 4662 4663 4672 4675 4675 4676 4684 4691 4701 4703 4708 4723 4729 4729 4731 4758 4771 4775 4782 4784 4788 4795 4795 4799 4825 4827 4830 4838 4842 4842 4849 4852 4857 4859 4864 4865 4868 4870 4885 4889 4889 4903 4914 4931 4937 4949 4962 4967 4981 4984 4987 4987 4989 4997 5004 5012 5015 5031 5032 5044 5063 5068 5073 5075 5080 5092 5097 5123 5130 5131 5144 5147 5156 5157 5159 5172 5185 5190 5201 5217 5219 5222 5228 5233 5233 5239 5241 5243 5245 5250 5251 5265 5273 5275 5277 5303 5305 5306 5316 5317 5322 5332 5337 5337 5354 5365 5372 5374 5375 5375 5383 5388 5399 5406 5414 5417 5442 5451 5452 5452 5460 5462 5493 5500 5507 5526 5533 5535 5545 5555 5558 5561 5561 5562 5562 5562 5577 5579 5580 5602 5606 5617 5623 5625 5643 5645 5650 5655 5662 5664 5664 5680 5683 5689 5691 5702 5705 5718 5729 5739 5739 5743 5748 5752 5756 5763 5765 5782 5789 5790 5792 5794 5795 5798 5803 5803 5813 5815 5829 5833 5834 5838 5849 5852 5854 5858 5861 5867 5873 5879 5880 5885 5887 5892 5900 5910 5912 5922 5923 5941 5942 5947 5956 5968 5972 5975 6001 6005 6008 6040 6041 6042 6044 6046 6047 6061 6063 6063 6065 6068 6070 6074 6074 6080 6086 6093 6093 6106 6111 6114 6118 6118 6121 6128 6135 6143 6151 6163 6174 6184 6193 6194 6198 6202 6203 6212 6215 6221 6221 6227 6236 6239 6243 6244 6251 6252 6254 6264 6264 6270 6274 6275 6281 6285 6290 6295 6305 6317 6324 6331 6336 6337 6348 6357 6365 6366 6371 6376 6381 6383 6388 6394 6395 6407 6413 6418 6420 6442 6449 6450 6454 6455 6465 6470 6496 6516 6517 6519 6525 6528 6528 6539 6541 6550 6551 6560 6568 6582 6584 6613 6623 6633 6645 6650 6655 6664 6664 6686 6689 6701 6707 6708 6711 6713 6717 6724 6739 6746 6776 6777 6791 6792 6802 6809 6819 6819 6831 6842 6844 6846 6853 6862 6867 6868 6879 6885 6893 6894 6896 6899 6901 6901 6902 6911 6912 6922 6923 6926 6930 6931 6935 6941 6944 6951 6957 6961 6967 6988 6996 7009 7019 7044 7049 7058 7062 7066 7069 7069 7098 7107 7111 7116 7120 7124 7131 7133 7135 7153 7153 7158 7173 7177 7184 7192 7211 7220 7237 7238 7261 7261 7272 7278 7301 7304 7306 7306 7312 7328 7333 7338 7342 7352 7369 7370 7373 7376 7388 7389 7393 7393 7394 7395 7397 7424 7426 7437 7440 7447 7461 7484 7486 7493 7495 7505 7507 7508 7512 7523 7526 7532 7538 7572 7572 7573 7574 7575 7594 7609 7611 7616 7616 7618 7620 7624 7628 7631 7647 7655 7662 7664 7665 7667 7672 7681 7683 7691 7692 7701 7717 7723 7734 7744 7759 7762 7763 7769 7772 7781 7783 7787 7793 7796 7798 7819 7820 7820 7822 7825 7836 7884 7888 7900 7902 7929 7938 7949 7950 7954 7960 7969 7971 7972 7986 7994 8012 8016 8023 8036 8037 8042 8054 8056 8058 8081 8084 8119 8124 8128 8133 8133 8137 8149 8153 8153 8169 8176 8184 8187 8188 8190 8197 8202 8207 8208 8223 8225 8227 8235 8239 8245 8254 8254 8257 8266 8273 8283 8289 8294 8300 8300 8301 8309 8310 8323 8326 8333 8340 8351 8363 8365 8368 8369 8370 8370 8378 8379 8383 8403 8410 8412 8422 8425 8429 8430 8439 8440 8442 8443 8454 8460 8472 8476 8483 8488 8489 8491 8494 8500 8500 8509 8514 8546 8547 8562 8578 8590 8594 8594 8595 8598 8601 8602 8603 8604 8607 8616 8624 8642 8644 8649 8657 8681 8690 8702 8703 8708 8709 8710 8720 8723 8756 8765 8774 8779 8783 8785 8814 8818 8822 8826 8836 8838 8845 8849 8859 8859 8872 8878 8882 8885 8912 8924 8930 8934 8943 8949 8954 8961 8964 8968 8990 9000 9015 9025 9038 9045 9045 9049 9050 9058 9063 9076 9076 9080 9107 9109 9111 9120 9124 9136 9137 9147 9150 9151 9159 9176 9187 9190 9193 9204 9232 9233 9251 9255 9260 9278 9281 9293 9296 9314 9316 9320 9326 9329 9329 9330 9331 9340 9341 9343 9346 9346 9347 9347 9349 9350 9371 9372 9375 9381 9385 9388 9390 9414 9418 9419 9420 9422 9429 9431 9435 9441 9442 9450 9461 9463 9463 9464 9465 9478 9482 9486 9511 9525 9528 9533 9536 9536 9540 9548 9553 9565 9576 9591 9595 9619 9625 9631 9650 9654 9659 9661 9664 9671 9675 9676 9690 9694 9700 9702 9709 9711 9726 9726 9732 9737 9742 9742 9743 9743 9749 9753 9760 9775 9778 9788 9795 9818 9831 9841 9841 9848 9854 9855 9856 9866 9868 9870 9878 9880 9881 9887 9888 9898 9898 9912 9917 9919 9927 9930 9937 9942 9960 9972 9979 9980 9983 9985 9985 9988 9990 9993
```

**Private Test Case 5**

**Input**

```
2 6 7 22 45 59 67 78 83 107 138 146 150 160 167 170 177 178 203 204 210 226 244 246 273 289 290 313 313 350 357 361 363 363 366 376 376 378 385 399 404 409 414 416 430 431 437 449 491 500 503 510 514 516 532 533 540 541 543 556 575 580 594 600 614 620 620 623 626 627 661 668 673 678 679 680 696 699 701 708 716 720 733 738 742 756 760 766 770 770 794 795 812 842 851 866 869 871 910 917 920 922 923 932 938 941 941 943 946 947 955 963 976 985 993 1010 1027 1028 1038 1047 1052 1081 1084 1091 1106 1121 1125 1133 1137 1149 1151 1155 1166 1167 1168 1198 1208 1212 1224 1227 1237 1240 1256 1265 1267 1281 1285 1293 1312 1314 1319 1322 1334 1336 1344 1358 1362 1391 1399 1404 1414 1426 1435 1440 1447 1448 1457 1461 1462 1462 1470 1476 1484 1490 1504 1509 1517 1523 1535 1540 1549 1549 1561 1565 1566 1569 1576 1584 1591 1606 1615 1618 1626 1629 1662 1667 1671 1675 1692 1693 1693 1713 1720 1729 1739 1767 1770 1798 1803 1806 1809 1824 1837 1844 1867 1880 1892 1893 1904 1905 1918 1918 1920 1937 1943 1947 1948 1964 1976 1979 1979 1999 2006 2029 2031 2038 2040 2042 2060 2069 2089 2093 2111 2114 2119 2123 2135 2146 2163 2176 2184 2188 2189 2199 2213 2247 2258 2270 2274 2279 2290 2295 2299 2311 2319 2320 2320 2327 2342 2358 2362 2364 2374 2385 2395 2404 2430 2437 2449 2461 2472 2484 2485 2489 2507 2508 2510 2515 2517 2517 2525 2530 2532 2534 2535 2544 2562 2575 2590 2592 2592 2599 2617 2617 2618 2625 2636 2636 2637 2650 2679 2686 2696 2700 2701 2714 2718 2722 2742 2743 2757 2759 2761 2764 2778 2792 2792 2814 2831 2832 2843 2845 2848 2858 2863 2868 2874 2879 2880 2884 2893 2897 2912 2953 2962 2967 2971 2995 2999 2999 3022 3033 3035 3059 3060 3065 3071 3074 3076 3090 3091 3104 3108 3113 3114 3127 3135 3142 3161 3165 3168 3170 3173 3194 3205 3211 3214 3216 3217 3229 3234 3237 3240 3240 3247 3247 3254 3258 3278 3284 3289 3291 3294 3298 3307 3325 3328 3336 3341 3351 3355 3355 3364 3364 3387 3389 3409 3417 3419 3419 3427 3431 3444 3450 3453 3455 3457 3474 3490 3500 3502 3518 3522 3527 3528 3531 3547 3560 3568 3571 3573 3573 3577 3578 3596 3609 3611 3619 3619 3643 3647 3650 3654 3654 3658 3672 3703 3707 3711 3727 3739 3744 3744 3749 3755 3758 3789 3792 3792 3798 3799 3816 3820 3825 3836 3841 3841 3848 3852 3862 3863 3878 3880 3897 3899 3910 3913 3922 3922 3924 3924 3926 3927 3931 3934 3937 3947 3953 3960 3971 3982 3990 4004 4004 4024 4046 4059 4072 4075 4080 4104 4106 4120 4120 4132 4136 4139 4145 4148 4187 4191 4192 4201 4204 4213 4231 4253 4257 4263 4272 4277 4277 4292 4301 4304 4305 4311 4317 4320 4322 4340 4368 4391 4414 4416 4429 4475 4477 4477 4511 4515 4530 4533 4535 4540 4542 4546 4546 4563 4565 4570 4573 4574 4578 4586 4594 4595 4610 4611 4615 4622 4622 4637 4639 4639 4647 4648 4656 4669 4671 4671 4673 4705 4707 4709 4716 4728 4745 4751 4762 4766 4771 4771 4777 4802 4803 4804 4806 4813 4814 4835 4835 4839 4843 4853 4857 4859 4873 4875 4881 4897 4901 4921 4928 4929 4945 4947 4954 4960 4999 5006 5008 5012 5014 5018 5033 5044 5057 5068 5072 5078 5094 5097 5098 5112 5123 5131 5151 5184 5204 5210 5226 5232 5233 5242 5244 5244 5246 5247 5249 5249 5256 5263 5277 5278 5291 5313 5325 5329 5332 5333 5335 5349 5357 5361 5362 5390 5391 5399 5399 5402 5420 5434 5446 5449 5469 5469 5486 5489 5503 5511 5524 5525 5526 5534 5534 5545 5549 5559 5563 5581 5584 5593 5595 5600 5605 5613 5631 5642 5648 5654 5655 5666 5679 5695 5709 5711 5714 5719 5738 5745 5761 5769 5770 5775 5789 5797 5803 5828 5836 5838 5851 5858 5875 5877 5879 5881 5887 5900 5921 5923 5927 5936 5944 5948 5966 5971 5972 5983 5995 5995 6002 6006 6008 6009 6025 6034 6044 6048 6057 6058 6058 6061 6068 6069 6073 6085 6092 6093 6103 6108 6113 6113 6122 6138 6163 6163 6166 6171 6189 6193 6205 6215 6227 6232 6234 6254 6255 6266 6273 6285 6290 6309 6312 6320 6330 6336 6347 6368 6378 6393 6399 6403 6414 6415 6424 6434 6443 6474 6493 6495 6503 6504 6506 6508 6531 6537 6544 6555 6561 6576 6592 6603 6610 6615 6616 6623 6623 6633 6656 6672 6683 6687 6719 6725 6727 6730 6733 6743 6745 6745 6747 6751 6760 6763 6772 6784 6786 6788 6788 6806 6812 6821 6837 6843 6850 6863 6867 6872 6877 6879 6882 6884 6897 6898 6913 6930 6940 6954 6962 6968 6971 6983 6989 7002 7011 7026 7031 7033 7043 7043 7058 7080 7104 7112 7113 7115 7129 7136 7152 7156 7160 7172 7172 7179 7185 7188 7196 7203 7205 7211 7213 7234 7245 7287 7293 7293 7295 7302 7302 7305 7318 7348 7354 7360 7370 7383 7385 7387 7399 7400 7409 7412 7415 7420 7420 7423 7423 7436 7439 7445 7460 7465 7483 7488 7507 7510 7517 7526 7532 7532 7551 7558 7586 7590 7592 7594 7599 7607 7608 7611 7627 7631 7643 7648 7652 7660 7684 7690 7700 7706 7708 7714 7721 7727 7757 7785 7795 7805 7819 7823 7836 7844 7845 7860 7879 7890 7897 7912 7922 7937 7951 7952 7954 7954 7962 7963 7987 7990 7993 8003 8027 8035 8041 8043 8056 8064 8066 8068 8068 8069 8072 8073 8080 8083 8096 8103 8110 8111 8153 8154 8158 8159 8182 8195 8195 8227 8237 8254 8266 8268 8269 8275 8278 8280 8288 8297 8303 8303 8306 8313 8327 8343 8344 8348 8362 8364 8378 8382 8383 8387 8387 8389 8402 8404 8407 8418 8433 8434 8448 8449 8454 8456 8461 8466 8470 8474 8480 8487 8488 8493 8496 8505 8505 8509 8520 8525 8529 8567 8569 8574 8575 8582 8592 8603 8604 8612 8614 8623 8652 8653 8654 8654 8676 8680 8697 8704 8707 8713 8719 8719 8721 8723 8731 8740 8754 8754 8764 8776 8795 8811 8812 8814 8822 8840 8851 8866 8867 8871 8873 8882 8899 8902 8913 8915 8924 8924 8931 8945 8946 8946 8959 8963 8977 8979 8995 9007 9021 9028 9038 9045 9051 9052 9069 9079 9079 9092 9107 9109 9129 9135 9147 9152 9159 9216 9221 9229 9252 9262 9283 9290 9293 9311 9316 9322 9349 9353 9362 9363 9384 9391 9398 9403 9410 9417 9477 9490 9492 9493 9495 9501 9504 9512 9541 9549 9556 9566 9567 9581 9581 9600 9606 9607 9614 9616 9620 9623 9630 9642 9657 9662 9682 9682 9683 9687 9692 9713 9727 9729 9738 9753 9779 9802 9804 9810 9819 9836 9843 9855 9873 9883 9897 9900 9914 9926 9932 9968 9976 9980 9985
8 13 14 20 21 24 24 29 29 44 47 54 63 78 90 100 113 114 118 121 126 127 128 134 137 139 145 170 173 176 180 195 210 211 216 217 218 220 222 222 224 233 242 249 269 270 273 287 288 295 296 311 315 315 326 332 333 336 337 337 339 353 365 367 389 389 394 399 402 411 413 416 417 423 440 441 442 446 455 455 465 468 489 495 502 508 514 516 516 525 527 528 529 533 542 545 552 556 557 562 565 567 583 597 601 604 609 612 614 624 629 635 645 647 647 649 649 655 655 663 667 674 680 685 688 688 690 706 714 718 720 722 731 737 742 757 763 772 776 782 784 788 791 799 799 801 803 805 813 823 824 832 848 850 856 862 866 867 881 884 889 890 891 891 901 909 926 934 947 955 961 964 973 982 982 987 993 996 998 999 1004 1009 1023 1027 1028 1049 1050 1065 1068 1068 1069 1086 1093 1093 1106 1109 1110 1114 1140 1148 1149 1159 1160 1167 1173 1179 1191 1191 1192 1195 1201 1234 1245 1263 1270 1279 1291 1295 1298 1304 1306 1309 1326 1335 1338 1364 1367 1371 1388 1393 1399 1402 1407 1408 1410 1410 1417 1428 1434 1438 1445 1446 1454 1467 1488 1491 1493 1496 1503 1506 1508 1511 1512 1530 1530 1536 1543 1548 1549 1550 1558 1558 1563 1567 1568 1569 1572 1574 1584 1586 1587 1589 1589 1598 1608 1608 1614 1618 1623 1634 1647 1648 1658 1660 1679 1690 1691 1696 1713 1713 1719 1720 1725 1736 1742 1754 1754 1755 1773 1779 1781 1783 1787 1799 1807 1810 1819 1828 1832 1839 1848 1850 1854 1859 1860 1878 1885 1889 1892 1894 1894 1900 1905 1907 1909 1924 1930 1941 1952 1956 1961 1963 1970 1970 1978 1984 1988 1988 1990 1996 2007 2010 2011 2018 2018 2018 2020 2024 2032 2044 2047 2053 2081 2088 2092 2097 2104 2106 2108 2112 2112 2121 2122 2126 2128 2130 2139 2144 2160 2166 2168 2174 2175 2178 2187 2201 2201 2201 2204 2212 2229 2232 2238 2239 2240 2248 2248 2255 2258 2266 2268 2286 2303 2303 2305 2311 2321 2335 2343 2344 2345 2347 2350 2355 2356 2362 2365 2371 2376 2380 2388 2389 2390 2395 2397 2412 2415 2415 2421 2423 2427 2428 2433 2433 2435 2438 2453 2458 2467 2469 2473 2474 2476 2482 2483 2486 2493 2495 2501 2517 2526 2535 2542 2544 2556 2566 2567 2579 2581 2589 2591 2596 2605 2605 2606 2611 2616 2633 2649 2651 2663 2668 2687 2688 2689 2694 2698 2705 2709 2709 2714 2718 2722 2726 2735 2736 2736 2763 2779 2781 2787 2788 2790 2811 2813 2814 2814 2837 2837 2850 2850 2853 2858 2862 2867 2873 2883 2896 2912 2919 2921 2928 2931 2938 2944 2949 2951 2951 2954 2968 2969 2971 2996 2997 2998 2998 3001 3006 3010 3017 3018 3020 3021 3025 3029 3031 3036 3041 3048 3050 3054 3058 3062 3068 3069 3076 3081 3091 3104 3104 3106 3121 3124 3124 3149 3151 3152 3169 3171 3171 3173 3177 3223 3229 3230 3231 3245 3250 3260 3260 3261 3274 3277 3280 3296 3304 3304 3313 3325 3327 3348 3365 3367 3371 3378 3384 3396 3399 3422 3423 3427 3441 3442 3452 3457 3459 3461 3466 3466 3500 3517 3517 3517 3524 3530 3533 3549 3552 3554 3565 3590 3596 3597 3599 3600 3602 3605 3616 3625 3625 3629 3636 3640 3644 3655 3657 3657 3670 3680 3687 3687 3689 3697 3698 3699 3703 3708 3710 3710 3714 3719 3725 3736 3742 3745 3747 3758 3759 3776 3784 3790 3794 3797 3797 3803 3805 3807 3815 3818 3819 3820 3825 3830 3842 3847 3847 3855 3858 3870 3872 3874 3881 3882 3890 3895 3907 3912 3913 3913 3944 3948 3952 3952 3957 3959 3960 3964 3972 3979 3983 3986 4002 4008 4026 4028 4030 4034 4039 4041 4045 4046 4054 4055 4082 4082 4086 4099 4106 4108 4109 4110 4111 4111 4114 4127 4132 4142 4144 4160 4172 4183 4185 4186 4192 4195 4202 4203 4214 4218 4232 4233 4243 4247 4254 4263 4264 4266 4268 4278 4281 4288 4293 4296 4300 4301 4307 4308 4310 4318 4318 4319 4332 4336 4339 4339 4341 4346 4350 4351 4355 4358 4361 4364 4369 4370 4370 4376 4395 4400 4405 4413 4432 4446 4449 4451 4452 4454 4455 4465 4468 4482 4483 4485 4488 4492 4496 4500 4500 4505 4515 4529 4537 4539 4542 4547 4557 4572 4572 4573 4578 4581 4582 4621 4624 4629 4635 4635 4642 4646 4663 4673 4680 4682 4683 4686 4701 4703 4705 4707 4715 4724 4724 4726 4728 4728 4730 4735 4736 4739 4744 4746 4749 4759 4762 4776 4780 4787 4793 4810 4838 4841 4851 4853 4855 4859 4860 4860 4862 4870 4879 4880 4893 4894 4937 4940 4942 4944 4955 4960 4963 4968 4973 4975 4977 4988 4998 5000 5005 5006 5015 5018 5022 5022 5023 5025 5031 5036 5037 5045 5051 5058 5060 5069 5072 5083 5084 5088 5089 5119 5123 5128 5134 5137 5138 5141 5144 5146 5151 5163 5167 5177 5195 5203 5217 5223 5233 5248 5249 5290 5297 5299 5304 5313 5319 5326 5330 5331 5343 5344 5349 5354 5354 5358 5361 5366 5376 5380 5387 5406 5418 5423 5425 5427 5429 5430 5434 5442 5453 5460 5471 5472 5482 5483 5486 5488 5491 5495 5496 5496 5498 5518 5518 5526 5539 5541 5544 5581 5596 5601 5605 5613 5631 5636 5661 5665 5676 5677 5686 5690 5692 5697 5707 5707 5711 5713 5725 5740 5750 5752 5774 5779 5782 5784 5796 5798 5808 5814 5817 5823 5834 5836 5838 5839 5841 5852 5868 5869 5876 5893 5908 5915 5918 5922 5931 5932 5934 5936 5950 5951 5971 5974 5975 5979 5982 5982 5983 5985 5996 5996 5997 6001 6001 6003 6003 6003 6005 6008 6020 6021 6021 6034 6042 6049 6058 6065 6069 6076 6089 6092 6098 6101 6104 6113 6126 6140 6145 6146 6151 6166 6168 6169 6171 6172 6173 6173 6173 6181 6181 6186 6192 6192 6208 6222 6222 6229 6239 6239 6248 6258 6259 6264 6266 6275 6286 6304 6315 6324 6326 6338 6339 6340 6343 6348 6352 6353 6362 6379 6383 6384 6385 6392 6414 6418 6428 6442 6453 6459 6465 6468 6478 6480 6482 6500 6503 6506 6509 6515 6517 6523 6523 6525 6537 6551 6556 6556 6577 6582 6584 6590 6595 6595 6605 6626 6644 6647 6648 6648 6650 6655 6661 6666 6674 6676 6682 6682 6682 6684 6692 6694 6701 6704 6709 6710 6711 6726 6740 6742 6756 6770 6771 6789 6809 6811 6827 6828 6831 6835 6844 6850 6850 6864 6884 6884 6884 6899 6905 6906 6909 6920 6924 6933 6941 6966 6968 6970 6979 6984 6987 6992 6995 7003 7007 7008 7021 7021 7023 7043 7047 7052 7062 7068 7072 7084 7106 7108 7113 7113 7116 7116 7119 7120 7121 7128 7131 7134 7142 7143 7148 7148 7149 7154 7159 7161 7170 7177 7182 7183 7191 7196 7197 7204 7205 7219 7230 7259 7263 7267 7269 7270 7273 7275 7280 7282 7283 7284 7292 7308 7309 7316 7318 7321 7323 7328 7332 7344 7349 7364 7367 7373 7385 7401 7405 7414 7418 7419 7439 7440 7450 7451 7453 7454 7458 7473 7482 7484 7491 7492 7494 7497 7509 7509 7517 7519 7529 7532 7538 7538 7554 7562 7567 7594 7602 7612 7632 7641 7652 7653 7659 7663 7683 7688 7695 7703 7705 7708 7720 7721 7734 7744 7756 7760 7765 7765 7772 7772 7774 7776 7777 7792 7806 7812 7819 7820 7823 7825 7825 7845 7852 7853 7864 7872 7872 7880 7880 7883 7885 7891 7899 7901 7910 7911 7914 7923 7936 7936 7939 7940 7942 7943 7946 7949 7949 7953 7955 7959 7959 7980 7982 7985 7987 7989 7990 7990 7995 8000 8011 8017 8020 8025 8028 8028 8029 8030 8036 8045 8052 8057 8071 8073 8084 8093 8094 8098 8102 8104 8104 8105 8106 8109 8110 8132 8145 8151 8152 8155 8163 8165 8178 8179 8185 8213 8219 8229 8231 8240 8242 8258 8262 8265 8277 8284 8287 8292 8295 8302 8320 8331 8339 8339 8340 8367 8376 8384 8393 8400 8400 8401 8407 8411 8418 8424 8425 8428 8443 8451 8458 8459 8465 8466 8482 8490 8502 8507 8507 8510 8516 8516 8524 8524 8532 8536 8547 8556 8560 8562 8563 8568 8583 8584 8591 8595 8617 8617 8617 8621 8624 8626 8650 8656 8682 8696 8708 8712 8714 8717 8719 8721 8725 8746 8755
```

**Output**

```
2 6 7 8 13 14 20 21 22 24 24 29 29 44 45 47 54 59 63 67 78 78 83 90 100 107 113 114 118 121 126 127 128 134 137 138 139 145 146 150 160 167 170 170 173 176 177 178 180 195 203 204 210 210 211 216 217 218 220 222 222 224 226 233 242 244 246 249 269 270 273 273 287 288 289 290 295 296 311 313 313 315 315 326 332 333 336 337 337 339 350 353 357 361 363 363 365 366 367 376 376 378 385 389 389 394 399 399 402 404 409 411 413 414 416 416 417 423 430 431 437 440 441 442 446 449 455 455 465 468 489 491 495 500 502 503 508 510 514 514 516 516 516 525 527 528 529 532 533 533 540 541 542 543 545 552 556 556 557 562 565 567 575 580 583 594 597 600 601 604 609 612 614 614 620 620 623 624 626 627 629 635 645 647 647 649 649 655 655 661 663 667 668 673 674 678 679 680 680 685 688 688 690 696 699 701 706 708 714 716 718 720 720 722 731 733 737 738 742 742 756 757 760 763 766 770 770 772 776 782 784 788 791 794 795 799 799 801 803 805 812 813 823 824 832 842 848 850 851 856 862 866 866 867 869 871 881 884 889 890 891 891 901 909 910 917 920 922 923 926 932 934 938 941 941 943 946 947 947 955 955 961 963 964 973 976 982 982 985 987 993 993 996 998 999 1004 1009 1010 1023 1027 1027 1028 1028 1038 1047 1049 1050 1052 1065 1068 1068 1069 1081 1084 1086 1091 1093 1093 1106 1106 1109 1110 1114 1121 1125 1133 1137 1140 1148 1149 1149 1151 1155 1159 1160 1166 1167 1167 1168 1173 1179 1191 1191 1192 1195 1198 1201 1208 1212 1224 1227 1234 1237 1240 1245 1256 1263 1265 1267 1270 1279 1281 1285 1291 1293 1295 1298 1304 1306 1309 1312 1314 1319 1322 1326 1334 1335 1336 1338 1344 1358 1362 1364 1367 1371 1388 1391 1393 1399 1399 1402 1404 1407 1408 1410 1410 1414 1417 1426 1428 1434 1435 1438 1440 1445 1446 1447 1448 1454 1457 1461 1462 1462 1467 1470 1476 1484 1488 1490 1491 1493 1496 1503 1504 1506 1508 1509 1511 1512 1517 1523 1530 1530 1535 1536 1540 1543 1548 1549 1549 1549 1550 1558 1558 1561 1563 1565 1566 1567 1568 1569 1569 1572 1574 1576 1584 1584 1586 1587 1589 1589 1591 1598 1606 1608 1608 1614 1615 1618 1618 1623 1626 1629 1634 1647 1648 1658 1660 1662 1667 1671 1675 1679 1690 1691 1692 1693 1693 1696 1713 1713 1713 1719 1720 1720 1725 1729 1736 1739 1742 1754 1754 1755 1767 1770 1773 1779 1781 1783 1787 1798 1799 1803 1806 1807 1809 1810 1819 1824 1828 1832 1837 1839 1844 1848 1850 1854 1859 1860 1867 1878 1880 1885 1889 1892 1892 1893 1894 1894 1900 1904 1905 1905 1907 1909 1918 1918 1920 1924 1930 1937 1941 1943 1947 1948 1952 1956 1961 1963 1964 1970 1970 1976 1978 1979 1979 1984 1988 1988 1990 1996 1999 2006 2007 2010 2011 2018 2018 2018 2020 2024 2029 2031 2032 2038 2040 2042 2044 2047 2053 2060 2069 2081 2088 2089 2092 2093 2097 2104 2106 2108 2111 2112 2112 2114 2119 2121 2122 2123 2126 2128 2130 2135 2139 2144 2146 2160 2163 2166 2168 2174 2175 2176 2178 2184 2187 2188 2189 2199 2201 2201 2201 2204 2212 2213 2229 2232 2238 2239 2240 2247 2248 2248 2255 2258 2258 2266 2268 2270 2274 2279 2286 2290 2295 2299 2303 2303 2305 2311 2311 2319 2320 2320 2321 2327 2335 2342 2343 2344 2345 2347 2350 2355 2356 2358 2362 2362 2364 2365 2371 2374 2376 2380 2385 2388 2389 2390 2395 2395 2397 2404 2412 2415 2415 2421 2423 2427 2428 2430 2433 2433 2435 2437 2438 2449 2453 2458 2461 2467 2469 2472 2473 2474 2476 2482 2483 2484 2485 2486 2489 2493 2495 2501 2507 2508 2510 2515 2517 2517 2517 2525 2526 2530 2532 2534 2535 2535 2542 2544 2544 2556 2562 2566 2567 2575 2579 2581 2589 2590 2591 2592 2592 2596 2599 2605 2605 2606 2611 2616 2617 2617 2618 2625 2633 2636 2636 2637 2649 2650 2651 2663 2668 2679 2686 2687 2688 2689 2694 2696 2698 2700 2701 2705 2709 2709 2714 2714 2718 2718 2722 2722 2726 2735 2736 2736 2742 2743 2757 2759 2761 2763 2764 2778 2779 2781 2787 2788 2790 2792 2792 2811 2813 2814 2814 2814 2831 2832 2837 2837 2843 2845 2848 2850 2850 2853 2858 2858 2862 2863 2867 2868 2873 2874 2879 2880 2883 2884 2893 2896 2897 2912 2912 2919 2921 2928 2931 2938 2944 2949 2951 2951 2953 2954 2962 2967 2968 2969 2971 2971 2995 2996 2997 2998 2998 2999 2999 3001 3006 3010 3017 3018 3020 3021 3022 3025 3029 3031 3033 3035 3036 3041 3048 3050 3054 3058 3059 3060 3062 3065 3068 3069 3071 3074 3076 3076 3081 3090 3091 3091 3104 3104 3104 3106 3108 3113 3114 3121 3124 3124 3127 3135 3142 3149 3151 3152 3161 3165 3168 3169 3170 3171 3171 3173 3173 3177 3194 3205 3211 3214 3216 3217 3223 3229 3229 3230 3231 3234 3237 3240 3240 3245 3247 3247 3250 3254 3258 3260 3260 3261 3274 3277 3278 3280 3284 3289 3291 3294 3296 3298 3304 3304 3307 3313 3325 3325 3327 3328 3336 3341 3348 3351 3355 3355 3364 3364 3365 3367 3371 3378 3384 3387 3389 3396 3399 3409 3417 3419 3419 3422 3423 3427 3427 3431 3441 3442 3444 3450 3452 3453 3455 3457 3457 3459 3461 3466 3466 3474 3490 3500 3500 3502 3517 3517 3517 3518 3522 3524 3527 3528 3530 3531 3533 3547 3549 3552 3554 3560 3565 3568 3571 3573 3573 3577 3578 3590 3596 3596 3597 3599 3600 3602 3605 3609 3611 3616 3619 3619 3625 3625 3629 3636 3640 3643 3644 3647 3650 3654 3654 3655 3657 3657 3658 3670 3672 3680 3687 3687 3689 3697 3698 3699 3703 3703 3707 3708 3710 3710 3711 3714 3719 3725 3727 3736 3739 3742 3744 3744 3745 3747 3749 3755 3758 3758 3759 3776 3784 3789 3790 3792 3792 3794 3797 3797 3798 3799 3803 3805 3807 3815 3816 3818 3819 3820 3820 3825 3825 3830 3836 3841 3841 3842 3847 3847 3848 3852 3855 3858 3862 3863 3870 3872 3874 3878 3880 3881 3882 3890 3895 3897 3899 3907 3910 3912 3913 3913 3913 3922 3922 3924 3924 3926 3927 3931 3934 3937 3944 3947 3948 3952 3952 3953 3957 3959 3960 3960 3964 3971 3972 3979 3982 3983 3986 3990 4002 4004 4004 4008 4024 4026 4028 4030 4034 4039 4041 4045 4046 4046 4054 4055 4059 4072 4075 4080 4082 4082 4086 4099 4104 4106 4106
4108 4109 4110 4111 4111 4114 4120 4120 4127 4132 4132 4136 4139 4142 4144 4145 4148 4160 4172 4183 4185 4186 4187 4191 4192 4192 4195 4201 4202 4203 4204 4213 4214 4218 4231 4232 4233 4243 4247 4253 4254 4257 4263 4263 4264 4266 4268 4272 4277 4277 4278 4281 4288 4292 4293 4296 4300 4301 4301 4304 4305 4307 4308 4310 4311 4317 4318 4318 4319 4320 4322 4332 4336 4339 4339 4340 4341 4346 4350 4351 4355 4358 4361 4364 4368 4369 4370 4370 4376 4391 4395 4400 4405 4413 4414 4416 4429 4432 4446 4449 4451 4452 4454 4455 4465 4468 4475 4477 4477 4482 4483 4485 4488 4492 4496 4500 4500 4505 4511 4515 4515 4529 4530 4533 4535 4537 4539 4540 4542 4542 4546 4546 4547 4557 4563 4565 4570 4572 4572 4573 4573 4574 4578 4578 4581 4582 4586 4594 4595 4610 4611 4615 4621 4622 4622 4624 4629 4635 4635 4637 4639 4639 4642 4646 4647 4648 4656 4663 4669 4671 4671 4673 4673 4680 4682 4683 4686 4701 4703 4705 4705 4707 4707 4709 4715 4716 4724 4724 4726 4728 4728 4728 4730 4735 4736 4739 4744 4745 4746 4749 4751 4759 4762 4762 4766 4771 4771 4776 4777 4780 4787 4793 4802 4803 4804 4806 4810 4813 4814 4835 4835 4838 4839 4841 4843 4851 4853 4853 4855 4857 4859 4859 4860 4860 4862 4870 4873 4875 4879 4880 4881 4893 4894 4897 4901 4921 4928 4929 4937 4940 4942 4944 4945 4947 4954 4955 4960 4960 4963 4968 4973 4975 4977 4988 4998 4999 5000 5005 5006 5006 5008 5012 5014 5015 5018 5018 5022 5022 5023 5025 5031 5033 5036 5037 5044 5045 5051 5057 5058 5060 5068 5069 5072 5072 5078 5083 5084 5088 5089 5094 5097 5098 5112 5119 5123 5123 5128 5131 5134 5137 5138 5141 5144 5146 5151 5151 5163 5167 5177 5184 5195 5203 5204 5210 5217 5223 5226 5232 5233 5233 5242 5244 5244 5246 5247 5248 5249 5249 5249 5256 5263 5277 5278 5290 5291 5297 5299 5304 5313 5313 5319 5325 5326 5329 5330 5331 5332 5333 5335 5343 5344 5349 5349 5354 5354 5357 5358 5361 5361 5362 5366 5376 5380 5387 5390 5391 5399 5399 5402 5406 5418 5420 5423 5425 5427 5429 5430 5434 5434 5442 5446 5449 5453 5460 5469 5469 5471 5472 5482 5483 5486 5486 5488 5489 5491 5495 5496 5496 5498 5503 5511 5518 5518 5524 5525 5526 5526 5534 5534 5539 5541 5544 5545 5549 5559 5563 5581 5581 5584 5593 5595 5596 5600 5601 5605 5605 5613 5613 5631 5631 5636 5642 5648 5654 5655 5661 5665 5666 5676 5677 5679 5686 5690 5692 5695 5697 5707 5707 5709 5711 5711 5713 5714 5719 5725 5738 5740 5745 5750 5752 5761 5769 5770 5774 5775 5779 5782 5784 5789 5796 5797 5798 5803 5808 5814 5817 5823 5828 5834 5836 5836 5838 5838 5839 5841 5851 5852 5858 5868 5869 5875 5876 5877 5879 5881 5887 5893 5900 5908 5915 5918 5921 5922 5923 5927 5931 5932 5934 5936 5936 5944 5948 5950 5951 5966 5971 5971 5972 5974 5975 5979 5982 5982 5983 5983 5985 5995 5995 5996 5996 5997 6001 6001 6002 6003 6003 6003 6005 6006 6008 6008 6009 6020 6021 6021 6025 6034 6034 6042 6044 6048 6049 6057 6058 6058 6058 6061 6065 6068 6069 6069 6073 6076 6085 6089 6092 6092 6093 6098 6101 6103 6104 6108 6113 6113 6113 6122 6126 6138 6140 6145 6146 6151 6163 6163 6166 6166 6168 6169 6171 6171 6172 6173 6173 6173 6181 6181 6186 6189 6192 6192 6193 6205 6208 6215 6222 6222 6227 6229 6232 6234 6239 6239 6248 6254 6255 6258 6259 6264 6266 6266 6273 6275 6285 6286 6290 6304 6309 6312 6315 6320 6324 6326 6330 6336 6338 6339 6340 6343 6347 6348 6352 6353 6362 6368 6378 6379 6383 6384 6385 6392 6393 6399 6403 6414 6414 6415 6418 6424 6428 6434 6442 6443 6453 6459 6465 6468 6474 6478 6480 6482 6493 6495 6500 6503 6503 6504 6506 6506 6508 6509 6515 6517 6523 6523 6525 6531 6537 6537 6544 6551 6555 6556 6556 6561 6576 6577 6582 6584 6590 6592 6595 6595 6603 6605 6610 6615 6616 6623 6623 6626 6633 6644 6647 6648 6648 6650 6655 6656 6661 6666 6672 6674 6676 6682 6682 6682 6683 6684 6687 6692 6694 6701 6704 6709 6710 6711 6719 6725 6726 6727 6730 6733 6740 6742 6743 6745 6745 6747 6751 6756 6760 6763 6770 6771 6772 6784 6786 6788 6788 6789 6806 6809 6811 6812 6821 6827 6828 6831 6835 6837 6843 6844 6850 6850 6850 6863 6864 6867 6872 6877 6879 6882 6884 6884 6884 6884 6897 6898 6899 6905 6906 6909 6913 6920 6924 6930 6933 6940 6941 6954 6962 6966 6968 6968 6970 6971 6979 6983 6984 6987 6989 6992 6995 7002 7003 7007 7008 7011 7021 7021 7023 7026 7031 7033 7043 7043 7043 7047 7052 7058 7062 7068 7072 7080 7084 7104 7106 7108 7112 7113 7113 7113 7115 7116 7116 7119 7120 7121 7128 7129 7131 7134 7136 7142 7143 7148 7148 7149 7152 7154 7156 7159 7160 7161 7170 7172 7172 7177 7179 7182 7183 7185 7188 7191 7196 7196 7197 7203 7204 7205 7205 7211 7213 7219 7230 7234 7245 7259 7263 7267 7269 7270 7273 7275 7280 7282 7283 7284 7287 7292 7293 7293 7295 7302 7302 7305 7308 7309 7316 7318 7318 7321 7323 7328 7332 7344 7348 7349 7354 7360 7364 7367 7370 7373 7383 7385 7385 7387 7399 7400 7401 7405 7409 7412 7414 7415 7418 7419 7420 7420 7423 7423 7436 7439 7439 7440 7445 7450 7451 7453 7454 7458 7460 7465 7473 7482 7483 7484 7488 7491 7492 7494 7497 7507 7509 7509 7510 7517 7517 7519 7526 7529 7532 7532 7532 7538 7538 7551 7554 7558 7562 7567 7586 7590 7592 7594 7594 7599 7602 7607 7608 7611 7612 7627 7631 7632 7641 7643 7648 7652 7652 7653 7659 7660 7663 7683 7684 7688 7690 7695 7700 7703 7705 7706 7708 7708 7714 7720 7721 7721 7727 7734 7744 7756 7757 7760 7765 7765 7772 7772 7774 7776 7777 7785 7792 7795 7805 7806 7812 7819 7819 7820 7823 7823 7825 7825 7836 7844 7845 7845 7852 7853 7860 7864 7872 7872 7879 7880 7880 7883 7885 7890 7891 7897 7899 7901 7910 7911 7912 7914 7922 7923 7936 7936 7937 7939 7940 7942 7943 7946 7949 7949 7951 7952 7953 7954 7954 7955 7959 7959 7962 7963 7980 7982 7985 7987 7987 7989 7990 7990 7990 7993 7995 8000 8003 8011 8017 8020 8025 8027 8028 8028 8029 8030 8035 8036 8041 8043 8045 8052 8056 8057 8064 8066 8068 8068 8069 8071 8072 8073 8073 8080 8083 8084 8093 8094 8096 8098 8102 8103 8104 8104 8105 8106 8109 8110 8110 8111 8132 8145 8151 8152 8153 8154 8155 8158 8159 8163 8165 8178 8179 8182 8185 8195 8195 8213 8219 8227 8229 8231 8237 8240 8242 8254 8258 8262 8265 8266 8268 8269 8275 8277 8278 8280 8284 8287 8288 8292 8295 8297 8302 8303 8303 8306 8313 8320 8327 8331 8339 8339 8340 8343 8344 8348 8362 8364 8367 8376 8378 8382 8383 8384 8387 8387 8389 8393 8400 8400 8401 8402 8404 8407 8407 8411 8418 8418 8424 8425 8428 8433 8434 8443 8448 8449 8451 8454 8456 8458 8459 8461 8465 8466 8466 8470 8474 8480 8482 8487 8488 8490 8493 8496 8502 8505 8505 8507 8507 8509 8510 8516 8516 8520 8524 8524 8525 8529 8532 8536 8547 8556 8560 8562 8563 8567 8568 8569 8574 8575 8582 8583 8584 8591 8592 8595 8603 8604 8612 8614 8617 8617 8617 8621 8623 8624 8626 8650 8652 8653 8654 8654 8656 8676 8680 8682 8696 8697 8704 8707 8708 8712 8713 8714 8717 8719 8719 8719 8721 8721 8723 8725 8731 8740 8746 8754 8754 8755 8764 8776 8795 8811 8812 8814 8822 8840 8851 8866 8867 8871 8873 8882 8899 8902 8913 8915 8924 8924 8931 8945 8946 8946 8959 8963 8977 8979 8995 9007 9021 9028 9038 9045 9051 9052 9069 9079 9079 9092 9107 9109 9129 9135 9147 9152 9159 9216 9221 9229 9252 9262 9283 9290 9293 9311 9316 9322 9349 9353 9362 9363 9384 9391 9398 9403 9410 9417 9477 9490 9492 9493 9495 9501 9504 9512 9541 9549 9556 9566 9567 9581 9581 9600 9606 9607 9614 9616 9620 9623 9630 9642 9657 9662 9682 9682 9683 9687 9692 9713 9727 9729 9738 9753 9779 9802 9804 9810 9819 9836 9843 9855 9873 9883 9897 9900 9914 9926 9932 9968 9976 9980 9985
```



## Week-2 Problem 3

You have a deck of shuffled cards ranging from 0 to 100,000,000. There are 2 sub-ordinate below you and two subordinates below them and it goes on.

- The job of the sub-ordinate is to split the deck of cards that they received and give it to two sub-ordinate of them. If they receive a deck of cards from their subordinates, they merge it in an ascending order and give it their higher level.

- If a subordinate received only two card, then he/she himself/herself arrange in ascending order give it back that to the superior.

- If a subordinate received only one card, then he/she will give back that to the superior.



```
Terminology:

(67) subordinate number 67

[1, 3, 5, 2] -> [1, 2, 3, 5] deck of cards got -> deck of cards returned

--------------------------------

(0) [3, 1, 2, 0, 5] -> [0, 1, 2, 3, 5]
|
|--(1) [3, 1] -> [1, 3]
|
|--(2) [2, 0, 5] -> [0, 2, 5]
    |
    |--(3) [2] -> [2]
    |
    |--(4) [0, 5] -> [0, 5]
```

Your task is to find how many people (including you) are required to sort the cards and print the sorted deck of cards and number of people required as a tuple.

Write the function **def subordinates(L):**

```python
def subordinates(L):
```

**Sample input 1**

```
[194, 69, 103, 150, 151, 44, 103, 98]
```

**Output**

```
([44, 69, 98, 103, 103, 150, 151, 194], 7)
```

**Sample input 2**

```
[10, 33, 45, 67, 92, 100, 5]
```

**Output**

```
([5, 10, 33, 45, 67, 92, 100], 7)
```



### Solution

**Solution Code**

```python
def merge(A, B):
  m, n = len(A), len(B)
  C, i, j, k = [], 0, 0, 0
  while k < m + n:
    if i == m:
      C.extend(B[j:])
      k = k + n - j
    elif j == n:
      C.extend(A[i:])
      k = k + m - i
    elif A[i] < B[j]:
      C.append(A[i])
      i, k = i + 1, k + 1
    else:
      C.append(B[j])
      j, k = j + 1, k + 1
  return C

def mergesort(L):
  global c
  c += 1
  n = len(L)
  if n == 2:
    return L if L[0] < L[1] else L[::-1]
  if n <= 1:
    return L
  m = n//2
  l = mergesort(L[:m])
  r = mergesort(L[m:])

  L_ = merge(l, r)

  return L_

def subordinates(L):
  return mergesort(L), c
c=0
```

**Suffix code(Visible)**

```python
print(subordinates(eval(input())))
```



**Public Testcase**

**Input 1**

```
[10, 33, 45, 67, 92, 100, 5, 99, 105]
```

**Output**

```
([5, 10, 33, 45, 67, 92, 99, 100, 105], 9)
```

**Input 2**

```
[194, 69, 103, 150, 151, 44, 103, 98]
```

**Output**

```
([44, 69, 98, 103, 103, 150, 151, 194], 7)
```

**Input 3**

```
[10, 33, 45, 67, 92, 100, 5]
```

**Output**

```
([5, 10, 33, 45, 67, 92, 100], 7)
```



**Private Testcase**

**Input 1**

```
[10, 33, 45, 67, 92, 100, 5, 99, 105, 26, 201]
```

**Output**

```
([5, 10, 26, 33, 45, 67, 92, 99, 100, 105, 201], 13)
```

**Input 2**

```
[10, 33, 45, 67, 92, 100, 5, 99, 105, 26]
```

**Output**

```
([5, 10, 26, 33, 45, 67, 92, 99, 100, 105], 11)
```

**Input 3**

```
[136, 152, 147, 3, 14, 43, 162, 169, 5, 10, 108, 182, 98, 71, 25, 122, 86, 82, 25, 176, 53, 14, 53, 85, 25, 110, 41, 5, 46, 168, 192, 140, 82, 109, 37, 165, 41, 146, 48, 123, 166, 17, 85, 87, 118, 133, 108, 170, 119, 198, 53, 154, 55, 193, 129, 189, 77, 69, 87, 80, 23, 58, 48, 79, 12, 4, 66, 30, 182, 118, 89, 140, 89, 15, 169, 101, 172, 0, 53, 24, 149, 168, 106, 41, 49, 148, 151, 88, 95, 192, 7, 101, 50, 40, 196, 72]
```

**Output**

```
([0, 3, 4, 5, 5, 7, 10, 12, 14, 14, 15, 17, 23, 24, 25, 25, 25, 30, 37, 40, 41, 41, 41, 43, 46, 48, 48, 49, 50, 53, 53, 53, 53, 55, 58, 66, 69, 71, 72, 77, 79, 80, 82, 82, 85, 85, 86, 87, 87, 88, 89, 89, 95, 98, 101, 101, 106, 108, 108, 109, 110, 118, 118, 119, 122, 123, 129, 133, 136, 140, 140, 146, 147, 148, 149, 151, 152, 154, 162, 165, 166, 168, 168, 169, 169, 170, 172, 176, 182, 182, 189, 192, 192, 193, 196, 198], 127)
```

**Input 4**

```
[147, 3, 14, 43, 162, 169, 5, 10, 108, 182, 98, 71, 25, 122, 86, 82, 25, 176, 53, 14, 53, 85, 25, 110, 41, 5, 46, 168, 192, 140, 82, 109, 37, 165, 41, 146, 48, 123, 166, 17, 85, 87, 118, 133, 108, 170, 119, 198, 53, 154, 55, 193, 129, 189, 77, 69, 87, 80, 23, 58, 48, 79, 12, 4, 66, 30, 182, 118, 89, 140, 89, 15, 169, 101, 172, 0, 53, 24, 149, 168, 106, 41, 49, 148, 151, 88, 95, 192, 7, 101, 50, 40]
```

**Output**

```
([0, 3, 4, 5, 5, 7, 10, 12, 14, 14, 15, 17, 23, 24, 25, 25, 25, 30, 37, 40, 41, 41, 41, 43, 46, 48, 48, 49, 50, 53, 53, 53, 53, 55, 58, 66, 69, 71, 77, 79, 80, 82, 82, 85, 85, 86, 87, 87, 88, 89, 89, 95, 98, 101, 101, 106, 108, 108, 109, 110, 118, 118, 119, 122, 123, 129, 133, 140, 140, 146, 147, 148, 149, 151, 154, 162, 165, 166, 168, 168, 169, 169, 170, 172, 176, 182, 182, 189, 192, 192, 193, 198], 119)
```





## Week-3 Problem 1

A restaurant always prepares dishes with the most orders before others with a lesser number of orders. Each dish in the restaurant menu has a unique integer ID. The restaurant receives `n` orders in a particular time period. The task is to find out the order of dish IDs according to which the restaurant will prepare them. Assume that restaurant has the following unique dish IDs in its menu:

`[1001, 1002, 1003, 1004, 1005, 1006, 1007, 1008, 1009]`

Write a function **DishPrepareOrder(order_list)** that accepts `order_list` in the form of a list of dish IDs and returns a list of dish IDs in the order in which the restaurant will prepare them. If two or more dishes have the same number of orders, then the dish which has a smaller ID value will be prepared first. 

**Sample input**

```
[1004,1003,1004,1003,1004,1005,1003,1004,1003,1002,1005,1002,1002,1001,1002,1002,1002]
```

**Output**

```
[1002, 1003, 1004, 1005, 1001]
```



### Solution	

```python
def insertionsort(L): #use this because it is stable sort
    n = len(L)
    if n < 1:
        return(L)
    for i in range(n):
        j = i
        while(j > 0 and L[j][1] > L[j-1][1]):
            (L[j],L[j-1]) = (L[j-1],L[j])
            j = j-1
    return(L)

def DishPrepareOrder(order_list):
    order_count = {}
    R = []
    for order in order_list:
        if order in order_count:
            order_count[order] += 1
        else:
            order_count[order] = 1
    for ID in sorted(order_count.keys()):
        R.append((ID,order_count[ID]))
    R=insertionsort(R)
    Res = []
    for tup in R:
        Res.append(tup[0])
    return Res
```

**Suffix Code(visible)**

```python
nums = eval(input())
print(DishPrepareOrder(nums))
```



**Public Test Cases**

Input 1

```
[1006, 1008, 1009, 1008, 1007, 1005, 1008, 1001, 1003, 1009, 1006, 1003, 1004, 1002, 1008, 1005, 1004, 1007, 1006, 1002, 1002, 1001, 1004, 1001, 1003, 1007, 1007, 1005, 1004, 1002]
```

Output

```
[1002, 1004, 1007, 1008, 1001, 1003, 1005, 1006, 1009]
```

Input 2

```
[1002, 1004, 1005, 1004, 1003, 1009, 1001, 1006, 1002, 1007, 1004, 1002, 1001, 1009, 1006, 1001, 1003, 1009, 1003, 1005, 1006, 1005, 1003, 1008, 1005, 1004, 1003, 1009, 1002, 1008, 1001, 1008, 1003, 1001, 1001, 1007, 1002, 1005, 1009, 1007, 1004, 1005, 1009, 1008, 1009, 1009, 1002, 1008, 1002, 1006, 1005, 1006, 1009, 1007, 1008, 1005, 1002, 1006, 1006, 1001, 1002, 1005, 1003, 1003, 1005, 1001, 1004, 1004, 1001, 1009, 1005, 1008, 1001, 1008, 1006, 1001, 1001, 1009, 1002, 1001, 1003, 1004, 1009, 1008, 1004, 1006, 1003, 1009, 1007, 1005, 1004, 1009, 1001, 1009, 1007, 1004, 1008, 1005, 1004, 1008]
```

Output

```
[1009, 1001, 1005, 1004, 1008, 1002, 1003, 1006, 1007]
```

Input 3

```
[1009, 1001, 1005, 1004, 1008, 1002, 1003, 1006, 1007]
```

Output

```
2[1001, 1002, 1003, 1004, 1005, 1006, 1007, 1008, 1009]
```



**Private Test Cases**

Input 1

```
[1003, 1002, 1004, 1005, 1005, 1008, 1006, 1001, 1002, 1001, 1003, 1006, 1006, 1006, 1007, 1002, 1009, 1003, 1006, 1005, 1008, 1006, 1005, 1006, 1002, 1003, 1006, 1004, 1004, 1006, 1004, 1001, 1004, 1005, 1002, 1008, 1001, 1003, 1009, 1003, 1006, 1001, 1009, 1001, 1008, 1001, 1009, 1007, 1002, 1004]
```

Output

```
[1006, 1001, 1002, 1003, 1004, 1005, 1008, 1009, 1007]
```

Input 2

```
[1006, 1007, 1005, 1006, 1003, 1009, 1001, 1005, 1007, 1007, 1004, 1008, 1002, 1002, 1006, 1001, 1006, 1003, 1001, 1006, 1007, 1002, 1005, 1003, 1007, 1007, 1001, 1008, 1007, 1005, 1003, 1005, 1001, 1005, 1003, 1003, 1004, 1001, 1007, 1006, 1006, 1007, 1006, 1008, 1009, 1004, 1005, 1002, 1006, 1002, 1002, 1003, 1007, 1007, 1004, 1006, 1006, 1001, 1004, 1004, 1006, 1007, 1008, 1006, 1004, 1002, 1008, 1002, 1008, 1006, 1007, 1007, 1006, 1008, 1002, 1004, 1004, 1008, 1005, 1001, 1007, 1003, 1005, 1007, 1006, 1001, 1002, 1004, 1001, 1008, 1001, 1007, 1007, 1001, 1005, 1003, 1009, 1007, 1004, 1006, 1001, 1002, 1005, 1006, 1003, 1003, 1006, 1001, 1005, 1003, 1005, 1005, 1004, 1006, 1005, 1002, 1005, 1006, 1005, 1003, 1006, 1006, 1001, 1003, 1009, 1009, 1005, 1003, 1008, 1005, 1006, 1008, 1004, 1002, 1008, 1001, 1009, 1008, 1009, 1009, 1003, 1007, 1009, 1005, 1003, 1006, 1008, 1007, 1001, 1008, 1009, 1006, 1004, 1002, 1004, 1006, 1001, 1002, 1007, 1008, 1003, 1002, 1004, 1008, 1009, 1006, 1002, 1004, 1004, 1006, 1001, 1001, 1006, 1002, 1009, 1005, 1004, 1005, 1009, 1006, 1008, 1001, 1009, 1001, 1002, 1004, 1003, 1003, 1007, 1001, 1006, 1006, 1006, 1001, 1001, 1005, 1003, 1001, 1001, 1007]
```

Output

```
[1006, 1001, 1007, 1005, 1003, 1004, 1002, 1008, 1009]
```

Input 3

```
[1006, 1007, 1004, 1005, 1007, 1008, 1001, 1006, 1002, 1006]
```

Output

```
[1006, 1007, 1001, 1002, 1004, 1005, 1008]
```

Input 4

```
[1006, 1003, 1005, 1009, 1008]
```

Output

```
[1003, 1005, 1006, 1008, 1009]
```

Input 5

```
[1003, 1003, 1003, 1003, 1003, 1003, 1003, 1003, 1003, 1003, 1003, 1003, 1003, 1003, 1003, 1003, 1003, 1003, 1003, 1003]
```

Output

```
[1003]
```

<div style="page-break-after: always; break-after: page;"></div>

## Week-3 Problem 2

A Stack is a linear data structure that follows the **LIFO (Last-In-First-Out)** principle. A stack can be defined as a container in which insertion (`push`) and deletion (`pop`) can be done only from one end.

For the given class **Stack**, create two methods:

* **push(data):** that accepts an integer `data` and inserts it into the stack in the last position.
* **pop():** if the stack is empty, return `Stack is empty` message. Otherwise, delete one element of the stack from the last position and returns deleted element value. 

**Note-** Use given linked list structure to implementation. Try to write both operations (`push` and `pop`) with **O(1)** time.

```python
class Node:
    def __init__(self, data):
        self.data = data # Stores data
        self.next = None # Contains the reference of next node
        self.prev = None # Contains the reference of previous node
class Stack:
    def __init__(self):
        self.head = None # Contains the reference of first node of the list
        self.last = None # Contains the reference of the last node of the list
```

**Example**

![](C:/Users/HP/Dropbox/PDSA/2022 PDSA JAN TERM/Week-3/Release with feedback/linklist.png)

**Sample Input**

```python
[1,3,5,7,9] # Elements for push in stack one by one, start from 1
2 # Perform pop 2 times from stack
```

**Output**

```python
9 # First popped element
7 # Second popped element
1, 3, 5 # Remaining elements in stack
```



### Solution

**Prefix code(visible)**

```python
class Node:
    def __init__(self, data):
        self.data = data
        self.next = None
        self.prev = None
class Stack:
    def __init__(self):
        self.head = None
        self.last = None 
```

**Solution code**

```python
    
    def push(self,data):
        newnode = Node(data)
        newnode.prev = self.last
        if self.head == None:            
            self.head = newnode
            self.last = newnode
        else:
            self.last.next = newnode
            self.last = newnode
    
    def pop(self):
        if(self.head != None):
            item = self.last.data
            if self.head == self.last:
                self.head=None
                self.last=None
            else:
                temp = self.last.prev
                temp.next = None
                self.last = temp
            return item
        else:
            return 'Stack is empty'
    
```

**Suffix code(visible)**

```python
    def traverse(self):
        temp = self.head
        while temp != None:
            if temp.next != None:
                print(temp.data, end=',')
            else:
                print(temp.data)
            temp = temp.next

ins = eval(input())
delt=int(input())
A = Stack()
for i in ins:
  A.push(i)
for j in range(delt):
  print(A.pop())
A.traverse()
```

**Public Test Cases**

Input 1

```
[1,3,5,7,9]
2
```

Output

```
9
7
1,3,5
```

Input 2

```
[1,2,3,4,5,6,7,8,9,10]
5
```

Output

```
10
9
8
7
6
1,2,3,4,5
```

Input 3

```
[1,2,3,4,5]
7
```

Output

```
5
4
3
2
1
Stack is empty
Stack is empty
```



**Private Test Cases**

Input 1

```
[1]
5
```

Output

```
1
Stack is empty
Stack is empty
Stack is empty
Stack is empty
```

Input 2

```
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42, 43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60, 61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 71, 72, 73, 74, 75, 76, 77, 78, 79, 80, 81, 82, 83, 84, 85, 86, 87, 88, 89, 90, 91, 92, 93, 94, 95, 96, 97, 98, 99]
10
```

Output

```
99
98
97
96
95
94
93
92
91
90
0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23,24,25,26,27,28,29,30,31,32,33,34,35,36,37,38,39,40,41,42,43,44,45,46,47,48,49,50,51,52,53,54,55,56,57,58,59,60,61,62,63,64,65,66,67,68,69,70,71,72,73,74,75,76,77,78,79,80,81,82,83,84,85,86,87,88,89
```

Input 3

```
[1,2]
1
```

Output

```
2
1
```

Input 4

```
[9,8,7,6,5,4,3,2,1]
8
```

Output

```
1
2
3
4
5
6
7
8
9
```

Input 5

```
[]
5
```

Output

```
Stack is empty
Stack is empty
Stack is empty
Stack is empty
Stack is empty
```

<div style="page-break-after: always; break-after: page;"></div>

## Week-3 Problem 3

**Quadratic probing** is an open addressing scheme in computer programming for resolving hash collisions in hash tables. Quadratic probing operates by taking the original hash index and adding successive values of an arbitrary quadratic polynomial until an open or empty slot is found.

An example of a sequence using quadratic probing is:

$h, h+1, h+4, h+9...h+i^2$

**Quadratic function**

Let *h*(*k*) be a hash function that maps an element *k* to an integer in [0, *m*−1], where *m* is the size of the table. Let the  $i^{th}$ probe position for a value *k* be given by the function

$h(k,i)= (h(k)+c_1i+c_2i^2)~mod~m$

where `h(k) = k mod m`, `c1 and c2` are positive integers. The value of `i = 0, 1, . . ., m – 1`. So we start from `i = 0`, and increase this until we get one free slot in hash table.



For a given class **Hashing**, create two methods:

* **store_data(data):** That accepts a positive integer `data` and generate an index value (0 to m-1) using given quadratic function and stores `data` in `hashtable` list on corresponding index generated by quadratic function. `hashtable` can contain only `m` data items. So, if all are already filled, then print `Hash table is full`.
* **display_hashtable():** That returns `hashtable`.

**Sample input**

```python
1 #c1
1 #c2
11 #m
[22,44,35,54,36,27] # data for store one by one, start from 22
```

**Output**

```python
[22, None, 44, 36, 35, 27, None, None, None, None, 54]
#index for 22 is 0
#index for 44 is 2
#index for 35 is 4
#index for 54 is 10
#index for 36 is 3
#index for 27 is 5
```



### Solution

**Prefix code(visible)**

```python
class Hashing:
  def __init__(self,c1,c2,m):
    self.hashtable = []
    for i in range(m):
        self.hashtable.append(None)     
    self.c1 = c1
    self.c2 = c2
    self.m = m
```

**Solution code**

```python
  def hashfunction(self,data):
    i = 0
    key = (data % self.m + self.c1*i + self.c2*(i**2)) % self.m
    while self.hashtable[key] != None and i < self.m: 
      key = (data % self.m + self.c1*i + self.c2*(i**2)) % self.m
      i = i + 1
    return key

  def store_data(self,data):
    if self.hashtable.count(None) != 0:
      key = self.hashfunction(data)
      self.hashtable[key]=data
    else:
      print('Hash table is full')

  def display_hashtable(self):
    return self.hashtable
```

**Suffix code(visible)**

```python
c1 = int(input())
c2 = int(input())
m = int(input())
data=eval(input())
A = Hashing(c1,c2,m)
for i in data:
	A.store_data(i)
print(A.display_hashtable())
```

**Public Test Cases**

Input 1

```
1
1
11
[22,44,35,54,36,27]
```

Output

```
[22, None, 44, 36, 35, 27, None, None, None, None, 54]
```

Input 2

```
1
1
11
[23,243,44,55,66]
```

Output

```
[44, 23, 55, 243, None, None, 66, None, None, None, None]
```

Input 3

```
1
2
11
[10,11,12,13,14,15,16,17,18,19,20,21]
```

Output

```
Hash table is full
[11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 10]
```



**Private Test Cases**

Input 1

```
1
1
11
[10,11,12,13,14,15,16,17,18,19,20]
```

Output

```
[11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 10]
```

Input 2

```
1
2
10
[25,35,45,65,85,78]
```

Output

```
[85, 65, None, None, None, 25, 45, None, 35, 78]
```

Input 3

```
3
4
21
[25,154,354,758,652,32,25]
```

Output

```
[None, 652, 758, None, 25, 25, None, 154, None, None, None, 32, None, None, None, None, None, None, 354, None, None]
```

Input 4

```
3
4
11
[22,22,22,22]
```

Output

```
[22, 22, None, None, None, None, None, 22, None, None, 22]
```

Input 5

```
3
4
6
[1,2,3,4,5,6,7,8,9]
```

Output

```
Hash table is full
Hash table is full
Hash table is full
[6, 1, 2, 3, 4, 5]
```

<div style="page-break-after: always; break-after: page;"></div>

## Week-4 Problem 1

**Long journey**

A tourist wants to travel around India from north to south. He has a policy that he never travels back towards the north. Write a Python function **`longJourney(AList)`** to find him a route with which he can visit the maximum number of cities according to his policy, where `AList` represents a graph of cities and routes between them.  Every edge in adjacency list `AList` is a feasible route between one city to another from north to south. The function should return a list in the order the cities are to be visited to visit maximum cities.

An example of cities and route between them(as edge) is shown below.  

![](C:/Users/HP/Dropbox/PDSA/2022 PDSA JAN TERM/Week-4/Release with feedback/longjourney.png)

**Sample Adjacency List**

```python
{'Madurai': ['Cochin', 'Kanyakumari'],
 'Vaishali': [],
 'Varanasi': ['Khajuraho', 'Bodhgaya'],
 'Thiruvanandhapuram': ['Kanyakumari'],
 'Udaipur': ['Gir', 'Ajanta'],
 'Rishikesh': ['Delhi'],
 'Shimla': ['Rishikesh'],
 'Bangalore': ['Chennai', 'Madurai'],
 'Agra': ['Ranthambore'],
 'Ellora': ['Aurangabad'],
 'Bodhgaya': ['Kolkatta'],
 'Cochin': ['Thiruvanandhapuram'],
 'Pushkar': ['Udaipur', 'Ranthambore'],
 'Ranthambore': ['Khajuraho'],
 'Gir': [],
 'Aurangabad': ['Mumbai'],
 'Kolkatta': ['Ajanta', 'Bangalore', 'Chennai'],
 'Chennai': ['Madurai'],
 'Sravasti': ['Kushinagar'],
 'Leh': ['Shimla'],
 'Sarnath': ['Varanasi'],
 'Delhi': ['Jaipur', 'Agra', 'Sravasti'],
 'Goa': ['Cochin', 'Bangalore'],
 'Kanyakumari': [],
 'Kushinagar': ['Sarnath', 'Vaishali'],
 'Khajuraho': ['Ajanta'],
 'Jaipur': ['Pushkar'],
 'Mumbai': ['Goa'],
 'Ajanta': ['Ellora', 'Aurangabad']}
 
 
```

**Sample Output**

```
['Leh', 'Shimla', 'Rishikesh', 'Delhi', 'Sravasti', 'Kushinagar', 'Sarnath', 'Varanasi', 'Bodhgaya', 'Kolkatta', 'Ajanta', 'Ellora', 'Aurangabad', 'Mumbai', 'Goa', 'Bangalore', 'Chennai', 'Madurai', 'Cochin', 'Thiruvanandhapuram', 'Kanyakumari']
```



### Solution

```python
# A Queue class to keep track of the visited nodes
class Queue:
  def __init__(self):
    self.queue = []

  def addq(self, v):
    self.queue.append(v)

  def delq(self):
    v = None
    if not self.isempty():
      v = self.queue[0]
      self.queue = self.queue[1:]
      return v

  def isempty(self):
    return self.queue == []

  def __str__(self):
    return str(self.queue)
 
# Dictionary inversion for d which has list as values
def dInv(d): 
  d_ = {}
  if not isinstance(list(d.values())[0], list):
    for k, v in d.items():
      if v not in d_:
        d_[v] = []
      d_[v].append(k)
    return d_

  if isinstance(list(d.values())[0], list):
    for k, v in d.items():
      for v_ in v:
        if v_ not in d_:
          d_[v_] = []
        d_[v_].append(k)
    return d_

# Longest path function from the lecture
def longestpathlist(AList): 
  indegree, lpath = {}, {}
  for u in AList:
    indegree[u], lpath[u] = 0, 0
  for u in AList:
    for v in AList[u]:
      indegree[v] = indegree[v] + 1
  
  zerodegreeq = Queue()
  for u in AList:
    if indegree[u] == 0:
      zerodegreeq.addq(u)
  
  while not zerodegreeq.isempty():
    j = zerodegreeq.delq()
    indegree[j] = indegree[j] - 1
    for k in AList[j]:
      indegree[k] = indegree[k] - 1
      lpath[k] = max(lpath[k], lpath[j] + 1)
      if indegree[k] == 0:
        zerodegreeq.addq(k)
  return lpath
  
def longJourney(AList):
  lpath = longestpathlist(AList) # longest path (dict)
  IAList = dInv(AList) # inverse adjacency list to get the reverse graph
  Ilj = dInv(lpath) # longest path as key and list of cities as values

  maxVal = max(lpath.values()) # value of longest path in which the city ends in the longest path
  prev = Ilj[maxVal][0] # last city
  path = [prev] # appending the last city
  for i in range(maxVal, -1, -1): # going backwards from last city to the first city in terms of longest path
    for p in Ilj[i]: # for every city p has the longest path i
      if p in IAList[prev]: 
        path.append(p) # append to path if there is edge from p to prev in AList or edge from prev to p in IAList(reversed graph)
        prev = p
  return path[::-1] # reverse the path since it is computed from the last
```

**Suffix (invisible)**

```python
AListString = ''
line = input()
while line.strip():
  AListString = AListString + '\n' + line
  line = input()
AList = eval(AListString)
path = longJourney(AList)
print(path)
```



**Public Test case 1**

**Input**

```
{'Madurai': ['Cochin', 'Kanyakumari'],
 'Vaishali': [],
 'Varanasi': ['Khajuraho', 'Bodhgaya'],
 'Thiruvanandhapuram': ['Kanyakumari'],
 'Udaipur': ['Gir', 'Ajanta'],
 'Rishikesh': ['Delhi'],
 'Shimla': ['Rishikesh'],
 'Bangalore': ['Chennai', 'Madurai'],
 'Agra': ['Ranthambore'],
 'Ellora': ['Aurangabad'],
 'Bodhgaya': ['Kolkatta'],
 'Cochin': ['Thiruvanandhapuram'],
 'Pushkar': ['Udaipur', 'Ranthambore'],
 'Ranthambore': ['Khajuraho'],
 'Gir': [],
 'Aurangabad': ['Mumbai'],
 'Kolkatta': ['Ajanta', 'Bangalore', 'Chennai'],
 'Chennai': ['Madurai'],
 'Sravasti': ['Kushinagar'],
 'Leh': ['Shimla'],
 'Sarnath': ['Varanasi'],
 'Delhi': ['Jaipur', 'Agra', 'Sravasti'],
 'Goa': ['Cochin', 'Bangalore'],
 'Kanyakumari': [],
 'Kushinagar': ['Sarnath', 'Vaishali'],
 'Khajuraho': ['Ajanta'],
 'Jaipur': ['Pushkar'],
 'Mumbai': ['Goa'],
 'Ajanta': ['Ellora', 'Aurangabad']}
 
 
```

**Output**

```
['Leh', 'Shimla', 'Rishikesh', 'Delhi', 'Sravasti', 'Kushinagar', 'Sarnath', 'Varanasi', 'Bodhgaya', 'Kolkatta', 'Ajanta', 'Ellora', 'Aurangabad', 'Mumbai', 'Goa', 'Bangalore', 'Chennai', 'Madurai', 'Cochin', 'Thiruvanandhapuram', 'Kanyakumari']
```

**Public Test case 2**

**Input**

```
{'Agra': ['Ranthambore'],
 'Delhi': ['Jaipur', 'Agra', 'Sravasti'],
 'Jaipur': ['Pushkar'],
 'Kushinagar': [],
 'Leh': ['Shimla'],
 'Pushkar': ['Ranthambore'],
 'Ranthambore': [],
 'Rishikesh': ['Delhi'],
 'Shimla': ['Rishikesh'],
 'Sravasti': ['Kushinagar']}


```

**Output**

```
['Leh', 'Shimla', 'Rishikesh', 'Delhi', 'Jaipur', 'Pushkar', 'Ranthambore']
```

**Public Test case 3**

**Input**

```
{'Agra': ['Ranthambore'],
 'Delhi': ['Jaipur', 'Agra', 'Sravasti'],
 'Jaipur': ['Pushkar'],
 'Kushinagar': ['Sarnath', 'Vaishali'],
 'Leh': ['Shimla'],
 'Pushkar': ['Ranthambore'],
 'Ranthambore': [],
 'Rishikesh': ['Delhi'],
 'Sarnath': ['Varanasi'],
 'Shimla': ['Rishikesh'],
 'Sravasti': ['Kushinagar'],
 'Vaishali': [],
 'Varanasi': []}


```

**Output**

```
['Leh', 'Shimla', 'Rishikesh', 'Delhi', 'Sravasti', 'Kushinagar', 'Sarnath', 'Varanasi']
```

**Private Test case 1**

**Input**

```
{'Madurai': ['Cochin', 'Kanyakumari'],
 'Vaishali': [],
 'Varanasi': ['Khajuraho', 'Bodhgaya'],
 'Thiruvanandhapuram': ['Kanyakumari'],
 'Udaipur': ['Gir', 'Ajanta'],
 'Rishikesh': ['Delhi'],
 'Shimla': ['Rishikesh'],
 'Bangalore': ['Chennai', 'Madurai'],
 'Agra': ['Ranthambore'],
 'Ellora': ['Aurangabad'],
 'Bodhgaya': ['Kolkatta'],
 'Cochin': ['Thiruvanandhapuram'],
 'Pushkar': ['Udaipur', 'Ranthambore'],
 'Ranthambore': ['Khajuraho'],
 'Gir': [],
 'Aurangabad': ['Mumbai'],
 'Kolkatta': ['Ajanta', 'Bangalore', 'Chennai'],
 'Chennai': ['Madurai'],
 'Sravasti': ['Kushinagar'],
 'Leh': ['Shimla'],
 'Sarnath': ['Varanasi'],
 'Delhi': ['Jaipur', 'Agra', 'Sravasti'],
 'Kanyakumari': [],
 'Kushinagar': ['Sarnath', 'Vaishali'],
 'Khajuraho': ['Ajanta'],
 'Jaipur': ['Pushkar'],
 'Mumbai': [],
 'Ajanta': ['Ellora', 'Aurangabad']}
 
 
```

**Output**

```
['Leh', 'Shimla', 'Rishikesh', 'Delhi', 'Sravasti', 'Kushinagar', 'Sarnath', 'Varanasi', 'Bodhgaya', 'Kolkatta', 'Bangalore', 'Chennai', 'Madurai', 'Cochin', 'Thiruvanandhapuram', 'Kanyakumari']
```

**Private Test case 2**

**Input**

```
{'Madurai': ['Cochin', 'Kanyakumari'],
 'Vaishali': [],
 'Varanasi': ['Khajuraho', 'Bodhgaya'],
 'Thiruvanandhapuram': ['Kanyakumari'],
 'Udaipur': ['Gir'],
 'Rishikesh': ['Delhi'],
 'Shimla': ['Rishikesh'],
 'Bangalore': ['Chennai', 'Madurai'],
 'Agra': ['Ranthambore'],
 'Bodhgaya': ['Kolkatta'],
 'Cochin': ['Thiruvanandhapuram'],
 'Pushkar': ['Udaipur', 'Ranthambore'],
 'Ranthambore': ['Khajuraho'],
 'Gir': [],
 'Kolkatta': ['Bangalore', 'Chennai'],
 'Chennai': ['Madurai'],
 'Sravasti': ['Kushinagar'],
 'Leh': ['Shimla'],
 'Sarnath': ['Varanasi'],
 'Delhi': ['Jaipur', 'Agra', 'Sravasti'],
 'Kanyakumari': [],
 'Kushinagar': ['Sarnath', 'Vaishali'],
 'Khajuraho': [],
 'Jaipur': ['Pushkar']}
 
 
```

**Output**

```
['Leh', 'Shimla', 'Rishikesh', 'Delhi', 'Sravasti', 'Kushinagar', 'Sarnath', 'Varanasi', 'Bodhgaya', 'Kolkatta', 'Bangalore', 'Chennai', 'Madurai', 'Cochin', 'Thiruvanandhapuram', 'Kanyakumari']
```

**Private Test case 3**

**Input**

```
{'Agra': ['Ranthambore'],
 'Ajanta': ['Ellora'],
 'Bangalore': ['Chennai', 'Madurai'],
 'Bodhgaya': ['Kolkatta'],
 'Chennai': ['Madurai'],
 'Cochin': ['Thiruvanandhapuram'],
 'Delhi': ['Jaipur', 'Agra', 'Sravasti'],
 'Ellora': [],
 'Gir': [],
 'Jaipur': ['Pushkar'],
 'Kanyakumari': [],
 'Khajuraho': ['Ajanta'],
 'Kolkatta': ['Ajanta', 'Bangalore', 'Chennai'],
 'Kushinagar': ['Sarnath', 'Vaishali'],
 'Leh': ['Shimla'],
 'Madurai': ['Cochin', 'Kanyakumari'],
 'Pushkar': ['Udaipur', 'Ranthambore'],
 'Ranthambore': ['Khajuraho'],
 'Rishikesh': ['Delhi'],
 'Sarnath': ['Varanasi'],
 'Shimla': ['Rishikesh'],
 'Sravasti': ['Kushinagar'],
 'Thiruvanandhapuram': ['Kanyakumari'],
 'Udaipur': ['Gir', 'Ajanta'],
 'Vaishali': [],
 'Varanasi': ['Khajuraho', 'Bodhgaya']}
 
 
```

**Output**

```
['Leh', 'Shimla', 'Rishikesh', 'Delhi', 'Sravasti', 'Kushinagar', 'Sarnath', 'Varanasi', 'Bodhgaya', 'Kolkatta', 'Bangalore', 'Chennai', 'Madurai', 'Cochin', 'Thiruvanandhapuram', 'Kanyakumari']
```

**Private Test case 4**

**Input**

```
{'Agra': ['Ranthambore'],
 'Ajanta': ['Ellora', 'Aurangabad'],
 'Aurangabad': ['Mumbai'],
 'Bangalore': ['Chennai', 'Madurai'],
 'Bodhgaya': ['Kolkatta'],
 'Chennai': ['Madurai'],
 'Delhi': ['Jaipur', 'Agra', 'Sravasti'],
 'Ellora': ['Aurangabad'],
 'Gir': [],
 'Goa': ['Bangalore'],
 'Jaipur': ['Pushkar'],
 'Kanyakumari': [],
 'Khajuraho': ['Ajanta'],
 'Kolkatta': ['Ajanta', 'Bangalore', 'Chennai'],
 'Kushinagar': ['Sarnath', 'Vaishali'],
 'Leh': ['Shimla'],
 'Madurai': ['Kanyakumari'],
 'Mumbai': ['Goa'],
 'Pushkar': ['Udaipur', 'Ranthambore'],
 'Ranthambore': ['Khajuraho'],
 'Rishikesh': ['Delhi'],
 'Sarnath': ['Varanasi'],
 'Shimla': ['Rishikesh'],
 'Sravasti': ['Kushinagar'],
 'Udaipur': ['Gir', 'Ajanta'],
 'Vaishali': [],
 'Varanasi': ['Khajuraho', 'Bodhgaya']}
 
 
```

**Output**

```
['Leh', 'Shimla', 'Rishikesh', 'Delhi', 'Sravasti', 'Kushinagar', 'Sarnath', 'Varanasi', 'Bodhgaya', 'Kolkatta', 'Ajanta', 'Ellora', 'Aurangabad', 'Mumbai', 'Goa', 'Bangalore', 'Chennai', 'Madurai', 'Kanyakumari']
```

**Private Test case 5**

**Input**

```
{'Agra': ['Ranthambore'],
 'Ajanta': ['Ellora', 'Aurangabad'],
 'Aurangabad': ['Mumbai'],
 'Bangalore': ['Chennai'],
 'Bodhgaya': ['Kolkatta'],
 'Chennai': [],
 'Delhi': ['Jaipur', 'Agra', 'Sravasti'],
 'Ellora': ['Aurangabad'],
 'Gir': [],
 'Goa': ['Bangalore'],
 'Jaipur': ['Pushkar'],
 'Khajuraho': ['Ajanta'],
 'Kolkatta': ['Ajanta', 'Bangalore', 'Chennai'],
 'Kushinagar': ['Sarnath', 'Vaishali'],
 'Leh': ['Shimla'],
 'Mumbai': ['Goa'],
 'Pushkar': ['Udaipur', 'Ranthambore'],
 'Ranthambore': ['Khajuraho'],
 'Rishikesh': ['Delhi'],
 'Sarnath': ['Varanasi'],
 'Shimla': ['Rishikesh'],
 'Sravasti': ['Kushinagar'],
 'Udaipur': ['Gir', 'Ajanta'],
 'Vaishali': [],
 'Varanasi': ['Khajuraho', 'Bodhgaya']}
 
 
```

**Output**

```
['Leh', 'Shimla', 'Rishikesh', 'Delhi', 'Sravasti', 'Kushinagar', 'Sarnath', 'Varanasi', 'Bodhgaya', 'Kolkatta', 'Ajanta', 'Ellora', 'Aurangabad', 'Mumbai', 'Goa', 'Bangalore', 'Chennai']
```



## Week-4 Problem 2

**Maze solver**

Alice wants to find the key in a maze and get out of it. The maze representation is given below, where `X` represents walls, *space* represents the allowed tiles Alice can walk on and `*` represents the tile that has the key. 

- There is only one tile opening in the left-most vertical wall, where Alice is initially standing.
- Similarly there is only one tile opening in the right-most vertical wall, from which Alice has to exit.
- Alice can travel horizontally or vertically, but cannot travel diagonally. Moving to adjacent tile vertically or horizontally is counted as a step.

There are three possible outcomes, either you can exit the maze after getting the key, or the key is not reachable or the finish tile is not reachable.

- Print the minimum number of steps Alice requires to reach the finish tile traveling through tile having the key. 
- If the key tile is not reachable then print `-1`.
- If the key tile is reachable but finish tile is not reachable then print `-2`.

**Note**: Input and printing are <u>required</u>

**Sample Input**

```
XXXXXXXXXXXXXX
   X    XXX  X
X  X    X X  X
X  X         X
X  XX  X XX  X
X  X  XX  X   
X     XX XXXXX
X  X         X
X  X      *  X
XXXXXXXXXXXXXX

```

**Sample Output**

```
31
```

### Solution

```python
# Create a graph with every tile as a vertex, with an edge betwwen adjacent
# tiles if Alice can travel between those tiles. 
def preprocessing(maze):
  m, n = len(maze), len(maze[0])
  S, E, K = None, None, None
  AList = {}
  for i in range(m):
    for j in range(n):
      AList[(i,j)] = []
      allowedTiles = [' ', '*']
      if maze[i][j] in allowedTiles:
        if i+1 < m and maze[i+1][j] in allowedTiles: 
          AList[(i,j)].append((i+1, j))
        if 0 <= i-1 and maze[i-1][j] in allowedTiles: 
          AList[(i,j)].append((i-1, j))
        if j+1 < n and maze[i][j+1] in allowedTiles:
          AList[(i,j)].append((i, j+1))
        if 0 <= j-1 and maze[i][j-1] in allowedTiles:
          AList[(i,j)].append((i, j-1))
        if j == 0: S = (i,j)
        if j == n-1: E = (i,j)
        if maze[i][j] == '*': K = (i,j)
  return AList, S, E, K

# Do a BFS maintaining level information to get the number of steps required.
def BFS(AList, x):
  visited = {k:False for k in AList.keys()}
  level = {k:None for k in AList.keys()}
  q = []

  visited[x] = True
  level[x] = 0
  q.append(x)
  while len(q) > 0:
    v = q.pop(0)
    visited[v] = True
    for i in AList[v]:
      if not visited[i]:
        q.append(i)
        if level[i] == None:
          level[i] = level[v] + 1
  from pprint import pprint
  return level

maze = []
line = input()
while line:
  maze.append(line)
  line = input()

AList, S, E, K = preprocessing(maze)
level = BFS(AList, S)
if level[K] == None:
  print(-1)
else:
  level2 = BFS(AList, K)
  if level2[E] == None:
    print(-2)
  else:
    print(level[K] + level2[E])
```

**Test cases**

**Public Test case 1**

**Input**

```
XXXXXXXXXXXXXX
   X    XXX  X
X  X    X X  X
X  X         X
X  XX  X XX  X
X  X  XX  X
X     XX XXXXX
X  X         X
X  X      *  X
XXXXXXXXXXXXXX


```

**Output**

```
31
```

**Public Test case 2**

**Input**

```
XXXXXXXXXXXXXX
   X    XXX  X
X  X    X X  X
X  X         X
X  XX  X XX  X
X  X  XX  X
X     XX XXXXX
X  X     X   X
X  X    X *  X
XXXXXXXXXXXXXX


```

**Output**

```
-1
```

**Public Test case 3**

**Input**

```
XXXXXXXXXXXXXX
   X    XXX  X
X  X    X X  X
X  X    X    X
X  XX  X XX  X
X  X  XX  X
X     XX XXXXX
X  X         X
X  X      *  X
XXXXXXXXXXXXXX


```

**Output**

```
-2
```

**Private Test case 1**

**Input**

```
XXXXXXXXXXXXXXXXX
X  X      X  X
X  X    X X  X  X
X  X         X  X
   XX  X X   X  X
X  X  XX *X X   X
X     XX  XX X  X
X  X    XX   X  X
X  X         X  X
XXXXXXXXXXXXXXXXX


```

**Output**

```
-2
```

**Private Test case 2**

**Input**

```
XXXXXXXXXXXXXXXXX
X  X      X  X
X  X    X X  X  X
X  X    X    X  X
   XX  X X   X  X
X  X  XX *X     X
X     XX  XX X  X
X  X    XX   X  X
X  X         X  X
XXXXXXXXXXXXXXXXX


```

**Output**

```
-1
```

**Private Test case 3**

**Input**

```
XXXXXXXXXXXXXX
X  X      X  X
X  X    X X  X
X  X    X    X
   XX  X XX  X
X  X  XX  X  *
X     XX XXXXX
X  X         X
X  X         X
XXXXXXXXXXXXXX


```

**Output**

```
24
```

**Private Test case 4**

**Input**

```
XXXXXXXXXXXXXX
X  X      X  X
X  X    X X  X
X  X    X    X
*  XX  X XX  X
X  X  XX  X   
X     XX XXXXX
X  X         X
X  X         X
XXXXXXXXXXXXXX


```

**Output**

```
24
```

**Private Test case 5**

**Input**

```
XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
X             XXXXXXXXXXXX       *      XXXXXXXXXXXXXX
X XXXXXXXXXXX XXXXXXXXXXXX XXXXXXXXXXXX XXXXXXXXXXXXXX
X XX      XXX X       XXXX XX        XX XX        XXXX
X XX XXXXX XX XX XXXXXX XX XX XXXXXXXXX XX XXXXXX XXXX
X XX XXXXX XX XX XXXXXX XX XX XXXXXXXXX XX XXXXXX XXXX
X XX XXXXX XX XX XXXXXX XX XX XXXXXXXXX XX XXXXXX XXXX
     XXXXX XX XX XXXXXX XX XX XXXXXXXXX XX XXXXXX XX
XXXX       XX XX XXXXXX XX XX        XX XX        XX X
XXXX XXXXXXXX XX XXXXXX XX XXXXXXXXX XX XX XXXXXX XX X
XXXX XXXXXXXX XX XXXXXX XX XXXXXXXXX XX XX XXXXXX XX X
XXXX XXXXXXXX XX XXXXXX XX XXXXXXXXX XX XX XXXXXX XX X
XXXX XXXXXXXX X       XXXX XX        XX XX XXXXXX XX X
XXXXXXXXXXXXX XXXXXXXXXXXX XXXXXXXXXXXX XXXXXXXXXXXX X
XXXXXXXXXXXXX              XXXXXXXXXXXX              X
XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX


```

**Output**

```
105
```

<div style="page-break-after: always;"></div>

## Week-4 Problem 3

Complete the Python function **`findAllPaths`** to find all possible paths from the source vertex to destination vertex in a directed acyclic graph. 

Function **findAllPaths(vertices, gList, source, destination)** takes `vertices` as a list of vertices, `gList` a dictionary that is an adjacency List representation of graph edges, `source` vertex, `destination` vertex, and returns a list of all paths from `source` to `destination`. The return value will be a List of Lists, where every path is a sequence of vertices as a List. Return an empty list if no path exists from 'source' to 'destination'.

``` python
def findAllPaths(vertices, gList, source, destination):
  # Your function definition goes here.
```



**Sample Input:**

```
vertices: [1, 2, 3, 4, 5, 6, 7, 8], 
gList: {1:[3,4], 2:[3], 3:[6], 4:[6,7], 5:[4,6], 6:[2], 7:[5]},
source: 1 
destination: 2
```

**Return:**

```
[[1, 3, 6, 2],
 [1, 4, 6, 2], 
 [1, 4, 7, 5, 6, 2]]
```

### Solution

```python
# Helper functions
from collections import deque
class myQueue:
  def __init__(self):
    self.Q = deque()
  
  def deQueue(self):
    return self.Q.popleft()

  def enQueue(self, x):
    return self.Q.append(x)

  def isEmpty(self):
    return False if self.Q else True

# Function  
def findAllPaths(vertices, gList, source , destination):
  allPaths=[]
  path=[]
  visited = {v:False for v in vertices}
  findAllPathsRecursive(vertices, gList, source, destination, visited, path, allPaths)
  return allPaths

# Function that will be called recursively to find path from original source to destination, that passes through vertex 'src'.
# If a path is found add it o allPaths.  
def findAllPathsRecursive(vertices, gList, src, dest, visited, path, allPaths):
  visited[src] = True
  path.append(src)

  if (src == dest):
    allPaths.append(path.copy())

  for e in gList[src]:
    if not visited[e]:
      findAllPathsRecursive(vertices, gList, e, dest, visited, path, allPaths)
    
  # If no path exist passing through this vertex remove it from path.
  # Mark it unvisited, this vertex could be part of some other path.
  path.pop()
  visited[src]=False
```

**Suffix**

```python
#Vertices are expected to be labelled as single letter or single digit 

#Sort and arrange the result for uniformity
def ArrangeResult(result):
  res = []
  for item in result:
    s = ""
    for i in item:
      s += str(i)    
    res.append(s)
  res.sort() 
  return res

v = [item for item in input().split(" ")]
Alist = {}
for i in v:
  Alist[i] = [item for item in input().split(" ") if item != '']
source = input()
dest = input()
print(ArrangeResult(findAllPaths(v, Alist, source, dest)))
```

**Public Test Case 1**

**Input**

```
1 2 3 4 5 6 7
3 4
3
6
6 7
4 6
2
5
1
2
```

**Output**

```
['1362', '1462', '147562']
```

**Public Test Case 2**

**Input**

```
a b c d e f g h i j
c
i
d g

f h
h




b
d
```

**Output**

```
[]
```

**Private Test Case 1**

**Input**

```
1 2 3 4 5 6 7
3 4
3
6
6 7
1 6
2
5
4
6
```

**Output**

```
['46', '475136', '4756']
```

**Private Test Case 2**

**Input**

```
a b c d e f g h i
c
i
d g

f h
h



g
h
```

**Output**

```
[]
```

**Private Test Case 3**

**Input**

```
a b c d e f g h i
c e
i
d g

f h
h

d

a
d
```

**Output**

```
['acd', 'aefhd', 'aehd']
```

**Private Test Case 4**

**Input**

```
a b c d e f g h i
c e
i
d g

f h
h
e
d g

a
d
```

**Output**

```
['acd', 'acgefhd', 'acgehd', 'aefhd', 'aehd']
```

**Private Test Case 5**

**Input**

```
a b c d e f g
b c d e f g
a c d e f g
a b d e f g
a b c e f g
a b c d f g
a b c d e g
a b c d e f
c
g
```

**Output**

```
['cabdefg', 'cabdeg', 'cabdfeg', 'cabdfg', 'cabdg', 'cabedfg', 'cabedg', 'cabefdg', 'cabefg', 'cabeg', 'cabfdeg', 'cabfdg', 'cabfedg', 'cabfeg', 'cabfg', 'cabg', 'cadbefg', 'cadbeg', 'cadbfeg', 'cadbfg', 'cadbg', 'cadebfg', 'cadebg', 'cadefbg', 'cadefg', 'cadeg', 'cadfbeg', 'cadfbg', 'cadfebg', 'cadfeg', 'cadfg', 'cadg', 'caebdfg', 'caebdg', 'caebfdg', 'caebfg', 'caebg', 'caedbfg', 'caedbg', 'caedfbg', 'caedfg', 'caedg', 'caefbdg', 'caefbg', 'caefdbg', 'caefdg', 'caefg', 'caeg', 'cafbdeg', 'cafbdg', 'cafbedg', 'cafbeg', 'cafbg', 'cafdbeg', 'cafdbg', 'cafdebg', 'cafdeg', 'cafdg', 'cafebdg', 'cafebg', 'cafedbg', 'cafedg', 'cafeg', 'cafg', 'cag', 'cbadefg', 'cbadeg', 'cbadfeg', 'cbadfg', 'cbadg', 'cbaedfg', 'cbaedg', 'cbaefdg', 'cbaefg', 'cbaeg', 'cbafdeg', 'cbafdg', 'cbafedg', 'cbafeg', 'cbafg', 'cbag', 'cbdaefg', 'cbdaeg', 'cbdafeg', 'cbdafg', 'cbdag', 'cbdeafg', 'cbdeag', 'cbdefag', 'cbdefg', 'cbdeg', 'cbdfaeg', 'cbdfag', 'cbdfeag', 'cbdfeg', 'cbdfg', 'cbdg', 'cbeadfg', 'cbeadg', 'cbeafdg', 'cbeafg', 'cbeag', 'cbedafg', 'cbedag', 'cbedfag', 'cbedfg', 'cbedg', 'cbefadg', 'cbefag', 'cbefdag', 'cbefdg', 'cbefg', 'cbeg', 'cbfadeg', 'cbfadg', 'cbfaedg', 'cbfaeg', 'cbfag', 'cbfdaeg', 'cbfdag', 'cbfdeag', 'cbfdeg', 'cbfdg', 'cbfeadg', 'cbfeag', 'cbfedag', 'cbfedg', 'cbfeg', 'cbfg', 'cbg', 'cdabefg', 'cdabeg', 'cdabfeg', 'cdabfg', 'cdabg', 'cdaebfg', 'cdaebg', 'cdaefbg', 'cdaefg', 'cdaeg', 'cdafbeg', 'cdafbg', 'cdafebg', 'cdafeg', 'cdafg', 'cdag', 'cdbaefg', 'cdbaeg', 'cdbafeg', 'cdbafg', 'cdbag', 'cdbeafg', 'cdbeag', 'cdbefag', 'cdbefg', 'cdbeg', 'cdbfaeg', 'cdbfag', 'cdbfeag', 'cdbfeg', 'cdbfg', 'cdbg', 'cdeabfg', 'cdeabg', 'cdeafbg', 'cdeafg', 'cdeag', 'cdebafg', 'cdebag', 'cdebfag', 'cdebfg', 'cdebg', 'cdefabg', 'cdefag', 'cdefbag', 'cdefbg', 'cdefg', 'cdeg', 'cdfabeg', 'cdfabg', 'cdfaebg', 'cdfaeg', 'cdfag', 'cdfbaeg', 'cdfbag', 'cdfbeag', 'cdfbeg', 'cdfbg', 'cdfeabg', 'cdfeag', 'cdfebag', 'cdfebg', 'cdfeg', 'cdfg', 'cdg', 'ceabdfg', 'ceabdg', 'ceabfdg', 'ceabfg', 'ceabg', 'ceadbfg', 'ceadbg', 'ceadfbg', 'ceadfg', 'ceadg', 'ceafbdg', 'ceafbg', 'ceafdbg', 'ceafdg', 'ceafg', 'ceag', 'cebadfg', 'cebadg', 'cebafdg', 'cebafg', 'cebag', 'cebdafg', 'cebdag', 'cebdfag', 'cebdfg', 'cebdg', 'cebfadg', 'cebfag', 'cebfdag', 'cebfdg', 'cebfg', 'cebg', 'cedabfg', 'cedabg', 'cedafbg', 'cedafg', 'cedag', 'cedbafg', 'cedbag', 'cedbfag', 'cedbfg', 'cedbg', 'cedfabg', 'cedfag', 'cedfbag', 'cedfbg', 'cedfg', 'cedg', 'cefabdg', 'cefabg', 'cefadbg', 'cefadg', 'cefag', 'cefbadg', 'cefbag', 'cefbdag', 'cefbdg', 'cefbg', 'cefdabg', 'cefdag', 'cefdbag', 'cefdbg', 'cefdg', 'cefg', 'ceg', 'cfabdeg', 'cfabdg', 'cfabedg', 'cfabeg', 'cfabg', 'cfadbeg', 'cfadbg', 'cfadebg', 'cfadeg', 'cfadg', 'cfaebdg', 'cfaebg', 'cfaedbg', 'cfaedg', 'cfaeg', 'cfag', 'cfbadeg', 'cfbadg', 'cfbaedg', 'cfbaeg', 'cfbag', 'cfbdaeg', 'cfbdag', 'cfbdeag', 'cfbdeg', 'cfbdg', 'cfbeadg', 'cfbeag', 'cfbedag', 'cfbedg', 'cfbeg', 'cfbg', 'cfdabeg', 'cfdabg', 'cfdaebg', 'cfdaeg', 'cfdag', 'cfdbaeg', 'cfdbag', 'cfdbeag', 'cfdbeg', 'cfdbg', 'cfdeabg', 'cfdeag', 'cfdebag', 'cfdebg', 'cfdeg', 'cfdg', 'cfeabdg', 'cfeabg', 'cfeadbg', 'cfeadg', 'cfeag', 'cfebadg', 'cfebag', 'cfebdag', 'cfebdg', 'cfebg', 'cfedabg', 'cfedag', 'cfedbag', 'cfedbg', 'cfedg', 'cfeg', 'cfg', 'cg']
```

<div style="page-break-after: always;"></div>

## Week-5 Problem 1

A courier company **XYZ** provides courier service between `n` cities labeled `0` to `n-1`, where customers can send items from any city to any another city. The company follows the shortest path to deliver items and charges `5 Rs.` per kilometer distance. The company wants to develop an inquiry system where customers can get the information on the cost and route for their courier.

Write a class **XYZ_Courier** that accepts a weighted adjacency list `Route_map` for an undirected and connected graph at the time of object creation in following format:-

```
Route_map = {
   	source_index : [(destination_index,distance),(destination_index,distance),..],
    ..
    ..
    source_index : [(destination_index,distance),(destination_index,distance),..]    
}
```

The class has following methods:-

* `cost(source, destination)` that accepts source name and destination name and returns minimum cost for delivery.

* `route(source, destination)` that accepts source name and destination name and returns the shortest route for delivery in the following format:

    `[source, place1, place2, ..., destination]`

    

**For the given graph**

![](C:/Users/HP/Dropbox/PDSA/2022 PDSA JAN TERM/Week-5/Release/img/prog3.jpg)

Sample input 1

```python
7 # number of vertices
[(0,1,10),(0,2,50),(0,3,300),(5,6,45),(2,1,30),(6,4,37),(1,6,65),(2,5,76),(1,3,40),(3,4,60),(2,4,20)] # edges
0 #source
4 #destination
```

Output

```python
300 #cost
[0, 1, 2, 4] #route
```

Sample input 2

```python
7
[(0,1,10),(0,2,50),(0,3,300),(5,6,45),(2,1,30),(6,4,37),(1,6,65),(2,5,76),(1,3,40),(3,4,60),(2,4,20)]
2
6
```

Output

```
285
[2, 4, 6]
```



### Solution

**Solution Code**

```python
class XYZ_Courier:
    def __init__(self,Route_map):
        self.Route_map = Route_map
    def dijkstra(self,WList,s):
        infinity = 1 + len(WList.keys())*max([d for u in WList.keys()for (v,d) in WList[u]])
        (visited,distance,prev) = ({},{},{})
        for v in WList.keys():
            (visited[v],distance[v],prev[v]) = (False,infinity,None)       
        distance[s] = 0    
        for u in WList.keys():
            nextd = min([distance[v] for v in WList.keys() if not visited[v]])
            nextvlist = [v for v in WList.keys() if (not visited[v]) and distance[v] == nextd]
            if nextvlist == []:
                break
            nextv = min(nextvlist)        
            visited[nextv] = True        
            for (v,d) in WList[nextv]:
                if not visited[v]:
                    if distance[v] > distance[nextv]+d:
                        distance[v] = distance[nextv]+d
                        prev[v] = nextv    
        return(distance,prev)  

    def cost(self,source,destination):
        distance,path = self.dijkstra(self.Route_map, source)
        return 5 * distance[destination]
  
    def route(self,source,destination):
        distance,path = self.dijkstra(self.Route_map, source)
        Route=[]
        if distance[destination]!=0:
            dest = destination
            while dest != source:
                Route = [dest] + Route
                for i,j in path.items():
                    if dest == i:
                        dest = j
                        break
            Route = [dest] + Route
        return Route
```

**Suffix Code**(visible)

```python
size = int(input())
edges = eval(input())
s=int(input())
d=int(input())
WL = {}
for i in range(size):
    WL[i] = []
for ed in edges: #for create list for undirected graph
    WL[ed[0]].append((ed[1],ed[2]))
    WL[ed[1]].append((ed[0],ed[2]))
C = XYZ_Courier(WL)
print(C.cost(s,d))
print(C.route(s,d))
```



 **Public Test case**

**Input 1**

```
7
[(0,1,10),(0,2,50),(0,3,300),(5,6,45),(2,1,30),(6,4,37),(1,6,65),(2,5,76),(1,3,40),(3,4,60),(2,4,20)]
0
4
```

**Output 1**

```
300
[0, 1, 2, 4]
```

**Input 2**

```
7
[(0,1,10),(0,2,50),(0,3,300),(5,6,45),(2,1,30),(6,4,37),(1,6,65),(2,5,76),(1,3,40),(3,4,60),(2,4,20)]
2
6
```

**Output 2**

```
285
[2, 4, 6]
```



**Input 3**

```
7
[(0,1,10),(0,2,50),(0,3,300),(5,6,45),(2,1,30),(6,4,37),(1,6,65),(2,5,76),(1,3,40),(3,4,60),(2,4,20)]
0
6
```

**Output 3**

```
375
[0, 1, 6]
```



**Private Test Case**

**Input 1**

```
7
[(0,1,10),(0,2,50),(0,3,300),(5,6,45),(2,1,30),(6,4,37),(1,6,65),(2,5,76),(1,3,40),(3,4,60),(2,4,20)]
3
5
```

**Output 1**

```
710
[3, 4, 6, 5]
```



**Input 2**

```
7
[(0,1,10),(0,2,20),(0,3,30),(5,6,120),(2,1,5),(6,4,20),(1,6,15),(2,5,70),(1,3,7),(3,4,100),(2,4,50)]
0
4
```

**Output 2**

```
225
[0, 1, 6, 4]
```



**Input 3**

```
7
[(0,1,10),(0,2,20),(0,3,30),(5,6,120),(2,1,5),(6,4,20),(1,6,15),(2,5,70),(1,3,7),(3,4,100),(2,4,50)]
0
5
```

**Output 3**

```
425
[0, 1, 2, 5]
```

**Input 4**

```
6
[(0,1,1),(0,2,6),(1,2,3),(1,3,4),(2,4,4),(2,3,2),(3,4,3),(1,5,2),(2,5,7),(3,5,1),(4,5,5)]
0
5
```

**Output 4**

```
15
[0, 1, 5]
```



<div style="page-break-after: always; break-after: page;"></div>

## Week-5 Problem 2

A taxi driver of an online cab service provider wants to go back to his home after dropping a customer. He wants to reduce the total cost (required for fuel, toll tax, etc.) to reach home by picking some customers. He checks the routes online. So, there are some routes available from his current location to his home location where he can earn money by picking some customers.

Write a function **min_cost(route_map, source, destination)** that accepts a weighted adjacency list `route_map` for a directed and connected graph of `n` vertices  (labeled from 0 to n-1) in the following format:-

```
route_map = {
   	source_index : [(destination_index,cost),(destination_index,cost),..],
    ..
    ..
    source_index : [(destination_index,cost),(destination_index,cost),..]    
}
```

**Note-** `cost` between two stops represents `Expenditure (on fuel, toll tax, etc) - Earning` so it may be negative. Assume that no negative weight cycle exists in the graph.

You are also given two integers `source` representing the current location of the taxi driver and `destination` representing the home location of the taxi driver. The function should returns the minimum cost route in the format `(minimum_cost, [source, next_stop, next_stop,.., destination])` from `source` to `destination`. 

**For the given graph**

![](C:/Users/HP/Dropbox/PDSA/2022 PDSA JAN TERM/Week-5/Release/img/image-20210922012821045.png)

Sample input 1

```python
8 #number of vertices
[(0,1,1000),(0,7,800),(1,5,200),(2,1,100),(2,3,100),(3,4,300),(4,5,500),(5,2,-200),(2,4,-200),(6,1,400),(6,5,100),(7,6,100)] # edges
0 #source
1 #destination
```

Output

```python
(900, [0, 7, 6, 5, 2, 1])
```

Sample input 2

```python
8
[(0,1,1000),(0,7,800),(1,5,200),(2,1,100),(2,3,100),(3,4,300),(4,5,500),(5,2,-200),(2,4,-200),(6,1,400),(6,5,100),(7,6,100)]
0
4
```

Output

```
(600, [0, 7, 6, 5, 2, 4])
```



### Solution

**Solution Code**

```python
def bellmanford(WList,s):
    infinity = 1 + len(WList.keys())*max([d for u in WList.keys() for (v,d) in WList[u]])
    distance = {}
    prev = {}
    for v in WList.keys():
        distance[v] = infinity
        prev[v] = None        
    distance[s] = 0 
    for i in WList.keys():
        for u in WList.keys():
            for (v,d) in WList[u]:
                if distance[v] > distance[u] + d:
                    distance[v] = distance[u] + d
                    prev[v] = u
    return (distance,prev)
     

def min_cost(route_map,source,destination):
    distance1,path1 = bellmanford(route_map, source)
    tot_dist = distance1[destination]
    Route_S_D = []
  # shortest route for source to destination
    if distance1[destination] != 0:
        dest = destination
        while dest != source:
            Route_S_D = [dest] + Route_S_D
            for i,j in path1.items():
                if dest == i:
                    dest = j
                    break
        Route_S_D = [dest] + Route_S_D
    return (tot_dist,Route_S_D)
```

**Suffix Code**(visible)

```python
size = int(input())
edges = eval(input())
s = int(input())
d = int(input())
WL = {}
for i in range(size):
    WL[i] = []
for ed in edges: #for create list for directed graph
    WL[ed[0]].append((ed[1],ed[2]))
print(min_cost(WL,s,d))
```

 **Public Test case**

**Input 1**

```
8
[(0,1,1000),(0,7,800),(1,5,200),(2,1,100),(2,3,100),(3,4,300),(4,5,500),(5,2,-200),(2,4,-200),(6,1,400),(6,5,100),(7,6,100)]
0
1
```

**Output 1**

```
(900, [0, 7, 6, 5, 2, 1])
```

**Input 2**

```
8
[(0,1,1000),(0,7,800),(1,5,200),(2,1,100),(2,3,100),(3,4,300),(4,5,500),(5,2,-200),(2,4,-200),(6,1,400),(6,5,100),(7,6,100)]
0
4
```

**Output 2**

```
(600, [0, 7, 6, 5, 2, 4])
```



**Input 3**

```
8
[(0,1,1000),(0,7,800),(1,5,200),(2,1,100),(2,3,100),(3,4,300),(4,5,500),(5,2,-200),(2,4,-200),(6,1,400),(6,5,100),(7,6,100)]
6
4
```

**Output 3**

```
(-300, [6, 5, 2, 4])
```



**Private Test Case**

**Input 1**

```
8
[(0,1,10),(0,3,20),(2,0,50),(1,3,-25),(3,2,-5),(4,1,70),(3,4,-30),(2,5,20),(5,3,-10),(4,6,10),(6,3,40),(5,7,50),(5,6,35),(6,7,15),(7,4,60)]
0
7
```

**Output 1**

```
(-20, [0, 1, 3, 4, 6, 7])
```



**Input 2**

```
8
[(0,1,1000),(0,7,800),(1,5,200),(2,1,100),(2,3,100),(3,4,300),(4,5,500),(5,2,-200),(2,4,-200),(6,1,400),(6,5,100),(7,6,100)]
0
2
```

**Output 2**

```
(800, [0, 7, 6, 5, 2])
```



**Input 3**

```
8
[(0,1,10),(0,3,20),(2,0,50),(1,3,-25),(3,2,-5),(4,1,70),(3,4,-30),(2,5,20),(5,3,-10),(4,6,10),(6,3,40),(5,7,50),(5,6,35),(6,7,15),(7,4,60)]
2
7
```

**Output 3**

```
(5, [2, 5, 3, 4, 6, 7])
```



**Input 4**

```
8
[(0,1,10),(0,3,20),(2,0,50),(1,3,-25),(3,2,-5),(4,1,70),(3,4,-30),(2,5,20),(5,3,-10),(4,6,10),(6,3,40),(5,7,50),(5,6,35),(6,7,15),(7,4,60)]
7
0
```

**Output 4**

```
(150, [7, 4, 1, 3, 2, 0])
```



## Week-5 Problem 3

An Internet service provider company wants to lay fiber lines to connect `n` cities labeled `0` to `n-1`. Write a function **FiberLink(distance_map)** that accepts a weighted adjacency list `distance_map` in the following format:-

```
distance_map = {
   	source_index : [(destination_index,distance(km)),(destination_index,distance),..],
    ..
    ..
    source_index : [(destination_index,distance),(destination_index,distance),..]    
}
```

The function returns the minimum length of fiber required to connect all `n` cities. 

**For the given graph**

![](C:/Users/HP/Dropbox/PDSA/2022 PDSA JAN TERM/Week-5/Release/img/prog3.jpg)

**Sample Input**

```python
7 # number of vertices
[(0,1,10),(0,2,50),(0,3,300),(5,6,45),(2,1,30),(6,4,37),(1,6,65),(2,5,76),(1,3,40),(3,4,60),(2,4,20)] #edges
```

Output

```python
182
```



### Solution

```python
def kruskal(WList):
    (edges,component,TE)=([],{},[])
    for u in WList.keys():
        edges.extend([(d,u,v) for (v,d) in WList[u]])
        component[u] = u
    edges.sort()
    for (d,u,v) in edges:
        if component[u] != component[v]:
            TE.append((u,v))
            c = component[u]
        for w in WList.keys():
            if component[w] == c:
                component[w] = component[v]
    return(TE)

def FiberLink(distance_map):
    R = kruskal(distance_map)
    S = 0
    for e in R:
        for ed in distance_map[e[0]]:
            if ed[0]==e[1]:
                S += ed[1]
    return S
```



**Suffix code**(visible)

``` python
size = int(input())
edges = eval(input())
WL = {}
for i in range(size):
    WL[i] = []
for ed in edges:
    WL[ed[0]].append((ed[1],ed[2]))
    WL[ed[1]].append((ed[0],ed[2]))
print(FiberLink(WL))
```

 **Public Test case**

**Input 1**

```
7
[(0,1,10),(0,2,50),(0,3,300),(5,6,45),(2,1,30),(6,4,37),(1,6,65),(2,5,76),(1,3,40),(3,4,60),(2,4,20)]
```

**Output 1**

```
182
```

**Input 2**

```
4
[(0,1,2),(1,2,4),(0,3,3),(0,2,1),(2,3,6)]
```

**Output 2**

```
6
```



**Input 3**

```
4
[(0,1,1),(1,2,2),(2,3,3),(0,3,4)]
```

**Output 3**

```
6
```



**Private Test Case**

**Input 1**

```
6
[(0,1,1),(0,2,6),(1,2,3),(1,3,4),(2,4,4),(2,3,2),(3,4,3),(1,5,2),(2,5,7),(3,5,1),(4,5,5)]
```

**Output 1**

```
9
```



**Input 2**

```
6
[(0,1,16),(0,3,2),(1,2,4),(3,4,10),(0,4,9),(3,5,15),(1,5,7),(2,5,6)]
```

**Output 2**

```
36
```



**Input 3**

```
6
[(0,1,10),(0,2,4),(1,2,7),(0,5,4),(0,4,2),(2,5,7),(3,5,2),(3,4,6)]
```

**Output 3**

```
19
```

**Input 4**

```
6
[(0,1,1),(0,2,4),(1,2,7),(0,5,14),(0,4,12),(2,5,17),(3,5,22),(3,4,26)]
```

**Output 4**

```
53
```



## Week-6 Problem 1

Write a Python function **KminGreaterThan(arr, k, num)**, that accepts an unsorted list `arr` , two numbers `k` and `num` , returns `True` if the `k`<sup> th</sup> smallest element in the list `arr` is greater than or equal to `num`, otherwise returns `False`. Try to write a solution that runs in $O(n\log{k})$ time.

```python
def KminGreaterThan(arr, k, num):
  #Complete this function
```

**Sample Input:** 

```
66 55 43 34 12 7 2 20 5
5
18
```

**Sample Output**

```
True
```

**Sample Input:** 

```
66 55 43 34 12 7 2 20 5
6
40
```

**Sample Output**

```
False
```

### Solution

```python
class max_heap:
  def __init__(self):
    self.size=0
    self.arr = []

  def isempty(self):
    return True if (self.size == 0) else False
  
  def max(self):
    return self.arr[0] if self.size>0 else None

  # Heapify element at index i
  def heapify_down(self, i):
    n = self.size
    a = self.arr
    while (i<n):
      gg = l = 2 * i + 1
      r = 2 * i + 2  
      # gg is greater of left and right child
      if (l<n and r<n and a[l] < a[r]):
        gg = r 
      if (gg<n and a[gg] > a[i]):
        a[i], a[gg] = a[gg], a[i]
        i = gg
      else:
        break

  def delete_max(self):
    max = self.arr[0]
    last = self.arr.pop()
    #size of heap after pop operation will reduce by 1
    self.size -= 1
    if (self.size > 0):
      self.arr[0] = last
      self.heapify_down(0)
    return max

  # Replace max element with x
  def replace_max(self, x):
    max = self.arr[0]
    self.arr[0] = x
    self.heapify_down(0)
    return max
  
  # Heapify last element in the heap 
  def heapify_up(self):
    i = self.size - 1
    while (i>0):
      parent = (i-1)//2
      if (self.arr[i] > self.arr[parent]):
        self.arr[i], self.arr[parent] = self.arr[parent], self.arr[i]
        i = parent
      else:
        break
  
  # Insert to min heap
  def insert_max_heap(self, x):
    self.arr.append(x)
    self.size += 1
    self.heapify_up()

# This function first finds the kth smallest element and than compares it with x 
def KminGreaterThan(arr, k, num):
  # Build max heap of size k
  h = max_heap()
  for i in range(k):
    h.insert_max_heap(L[i])

  # Insert the next element in the list if it is smaller than the max in heap.
  # This will ensure that the heap contains k minimum elements from the parsed elements.
  for i in range(k, len(arr)):
    if (h.max() > arr[i]):
      h.replace_max(arr[i])
  return True if (h.max() >= num) else False
```

**Another good solution**

```python
def min_heapify(li,i):
  lchild = 2*i + 1
  rchild = 2*i + 2
  small = i
  if lchild < len(li) and li[lchild] < li[small]: 
    small = lchild
  if rchild < len(li) and li[rchild] < li[small]: 
    small = rchild 
  if small != i:
    (li[small],li[i]) = (li[i], li[small])
    min_heapify(li,small)
def create_heap(li):
  for x in range((len(li)//2)-1,-1,-1):
    min_heapify(li,x)
  return li
def delete_root(li): 
  li[-1],li[0] = li[0],li[-1]
  temp = li.pop()
  min_heapify(li,0) 
  return temp
def KminGreaterThan(arr,k,num):
  arr = create_heap(arr)
  for x in range(k): 
    deleted_val = delete_root(arr)
  return deleted_val > num
```



**Suffix** 

```python
L = [int(item) for item in input().split(" ")]
k = int(input())
num = int(input())
print(KminGreaterThan(L, k, num))
```

<div style="page-break-after: always;"></div>

Public Test Case 1

input 

```
1 10 11 12 15 23 31 34 61 64 65 72 84 92 96
4
30
```

output

```
False
```

Public Test case 2

input

```
77 97 109 147 155 160 188 191 205 206 217 242 252 286 289 294 308 312 335 375 393 402 405 455 456
10
78
```

output

```
True
```

Private Test case 1

input

```
80 98 110 117 125 126 127 132 150 159 171 183 187 200 208 209 221 223 236 242 247 255 274 283 315 340 346 356 361 365
11
150
```

output

```
True
```

Private Test case 2

input

```
63 71 93 118 125 129 136 147 149 159 166 172 173 183 184 185 220 226 236 238 251 279 290 298 302 327 340 343 360 366 396 406 407 413 439 454 460 474 487 488 494 498 543 549 550 560 563 582 587 600
23
256
```

output

```
True
```

Private Test case 3

input

```
77 97 109 147 155 160 188 191 205 206 217 242 252 286 289 294 308 312 335 375 393 402 405 455 456
10
78
```

output

```
True
```

Private Test case 4

input

```
60074 60077 60078 60084 60114 60121 60122 60124 60129 60152 60153 60156 60183 60197 60201 60203 60220 60227 60228 60234 60244 60265 60266 60272 60280 60292 60298 60301 60304 60308 60313 60317 60323 60329 60334 60335 60356 60359 60361 60372 60377 60379 60391 60403 60406 60407 60408 60409 60416 60420 60423 60459 60466 60469 60474 60478 60483 60486 60491 60518 60526 60539 60542 60549 60557 60577 60592 60598 60599 60605 60610 60623 60625 60628 60639 60645 60651 60698 60746 60748 60749 60759 60760 60766 60770 60772 60777 60788 60789 60808 60819 60829 60830 60844 60852 60863 60882 60884 60900 60906 60908 60910 60916 60924 60927 60931 60935 60945 60970 60975 60976 60980 61001 61002 61020 61042 61045 61071 61081 61129 61136 61138 61146 61157 61158 61164 61168 61179 61191 61220 61232 61242 61254 61271 61272 61282 61297 61356 61372 61376 61383 61388 61406 61411 61422 61440 61445 61465 61476 61509 61516 61540 61557 61580 61582 61616 61626 61628 61629 61644 61645 61648 61668 61670 61685 61686 61687 61711 61741 61743 61759 61780 61785 61795 61808 61822 61832 61844 61852 61867 61869 61878 61896 61914 61921 61932 61944 61951 61952 61975 61979 61986 62000 62006 62011 62017 62031 62036 62040 62041 62048 62052 62057 62069 62076 62078 62079 62103 62134 62187 62189 62203 62204 62207 62209 62213 62214 62225 62227 62266 62272 62278 62298 62301 62302 62304 62327 62333 62345 62347 62363 62405 62406 62426 62427 62444 62457 62470 62485 62490 62511 62513 62524 62530 62532 62542 62544 62546 62549 62553 62554 62557 62559 62561 62589 62596 62597 62598 62615 62619 62631 62643 62653 62654 62659 62662 62666 62693 62703 62727 62731 62733 62744 62746 62747 62752 62754 62759 62767 62770 62772 62774 62777 62806 62816 62819 62829 62841 62859 62875 62918 62926 62927 62940 62948 62954 62960 62994 63003 63005 63006 63008 63016 63024 63025 63034 63036 63052 63053 63062 63074 63078 63079 63086 63088 63095 63098 63105 63106 63111 63118 63120 63126 63139 63167 63169 63174 63191 63194 63195 63198 63203 63208 63260 63262 63263 63265 63275 63293 63317 63323 63334 63345 63346 63350 63351 63362 63364 63394 63400 63405 63408 63411 63413 63429 63435 63437 63447 63457 63459 63488 63505 63507 63515 63559 63569 63572 63575 63585 63586 63589 63604 63617 63622 63623 63629 63631 63641 63643 63680 63686 63691 63692 63699 63702 63704 63705 63710 63714 63722 63739 63742 63757 63780 63786 63788 63794 63799 63812 63814 63820 63823 63828 63834 63837 63849 63853 63866 63877 63879 63881 63891 63908 63910 63916 63921 63927 63931 63944 63954 63960 63969 63984 64016 64023 64027 64035 64038 64042 64056 64060 64065 64074 64107 64116 64123 64137 64141 64161 64192 64193 64197 64199 64235 64236 64243 64251 64252 64274 64286 64293 64300 64317 64329 64342 64355 64359 64380 64381 64418 64461 64462 64472 64479 64487 64498 64509 64519 64529 64532 64542 64544 64573 64580 64600 64604 64608 64619 64624 64635 64640 64708 64718 64725 64734 64773 64787 64802 64804 64809 64810 64818 64853 64871 64873 64874 64885 64894 64895 64896 64907 64914 64929 64973 64975 64982 64993 65005 65017 65026 65033 65050 65056 65062 65063 65065 65067 65098 65128 65130 65150 65151 65174 65175 65190 65192 65202 65222 65242 65259 65268 65271 65278 65284 65291 65293 65295 65296 65300 65312 65316 65318 65325 65326 65331 65360 65380 65395 65416 65422 65433 65453 65460 65465 65478 65510 65513 65514 65529 65531 65536 65537 65559 65576 65583 65603 65612 65615 65628 65636 65640 65643 65659 65677 65685 65702 65706 65710 65712 65717 65728 65734 65756 65768 65780 65788 65797 65798 65803 65812 65822 65835 65838 65854 65861 65864 65867 65870 65880 65912 65919 65927 65934 65940 65942 65946 65952 65953 65963 65965 65971 65981 65987 65990 65998 66015 66029 66067 66080 66084 66095 66100 66104 66117 66129 66155 66157 66177 66191 66207 66215 66222 66223 66234 66254 66281 66287 66331 66341 66350 66370 66375 66376 66384 66388 66391 66397 66409 66410 66413 66423 66430 66433 66439 66442 66448 66467 66468 66472 66482 66506 66507 66514 66516 66522 66523 66543 66553 66554 66557 66574 66588 66590 66595 66624 66633 66645 66648 66659 66662 66677 66688 66695 66697 66700 66708 66710 66713 66720 66732 66735 66737 66758 66777 66790 66803 66820 66830 66856 66870 66878 66898 66908 66915 66935 66953 66967 67004 67019 67023 67027 67031 67061 67065 67068 67087 67123 67138 67151 67156 67168 67187 67189 67207 67213 67214 67215 67244 67255 67265 67273 67277 67294 67304 67315 67324 67338 67353 67359 67370 67383 67388 67400 67409 67415 67421 67433 67438 67450 67462 67477 67480 67489 67505 67527 67534 67577 67587 67589 67596 67599 67607 67614 67616 67618 67620 67632 67634 67637 67661 67668 67684 67686 67689 67707 67710 67713 67718 67728 67769 67773 67780 67790 67794 67810 67843 67845 67877 67899 67904 67929 67935 67943 67949 67954 68000 68004 68005 68008 68014 68029 68031 68046 68058 68059 68075 68076 68103 68109 68122 68147 68160 68168 68180 68193 68209 68226 68235 68239 68252 68262 68269 68275 68283 68297 68320 68324 68344 68351 68356 68379 68381 68391 68399 68402 68403 68405 68449 68453 68454 68456 68466 68473 68475 68479 68484 68492 68504 68525 68527 68531 68534 68535 68540 68548 68588 68589 68601 68614 68635 68640 68642 68669 68694 68698 68700 68703 68704 68725 68730 68731 68734 68735 68767 68785 68792 68810 68821 68822 68835 68837 68841 68848 68859 68869 68880 68886 68888 68899 68907 68916 68920 68921 68923 68925 68940 68948 68952 68961 68991 68992 68997 69000 69004 69008 69012 69013 69015 69020 69039 69051 69062 69063 69085 69090 69091 69101 69113 69123 69133 69153 69154 69157 69170 69176 69185 69191 69196 69206 69209 69223 69280 69282 69292 69295 69309 69326 69329 69332 69345 69347 69380 69383 69385 69390 69405 69411 69432 69436 69437 69455 69466 69467 69472 69497 69499 69523 69533 69541 69546 69549 69561 69570 69574 69588 69592 69597 69617 69672 69673 69685 69691 69696 69705 69706 69708 69710 69717 69727 69738 69741 69742 69774 69780 69784 69786 69788 69798 69799 69810 69813 69819 69822 69829 69840 69846 69849 69863 69872 69873 69888 69898 69914 69928 69945 69961 69963 69985 69987 69993 69995 70005 70026 70029 70033 70050 70063 70072 70084 70091 70122 70124 70148 70151 70164 70167 70177 70179 70187 70203 70212 70231 70232 70239 70255 70259 70267 70299 70302 70305 70308 70312 70315 70324 70338 70344 70349 70356 70366 70380 70390 70400 70412 70414 70419 70424 70433 70446 70462 70468 70470 70479 70511 70523 70528 70539 70541 70546 70552 70567 70588 70590 70599 70614 70619 70621 70658 70664 70678 70681 70691 70696 70728 70730 70746 70769 70771 70777 70781 70784 70788 70793 70810 70820 70825 70858 70882 70896 70917 70923 70929 70931 70960 70964 70979 70989 71015 71022 71023 71044 71072 71083 71088 71093 71094 71098 71119 71128 71136 71139 71146 71160 71167 71169 71186 71189 71198 71200 71204 71216 71230 71241 71261 71277 71301 71302 71310 71314 71326 71328 71336 71352 71353 71354 71361 71362 71364 71368 71373 71383 71384 71397 71400 71405 71411 71455 71457 71471 71493 71509 71510 71513 71515 71527 71531 71532 71562 71585 71589 71602 71604 71608 71627 71629 71641 71643 71651 71653 71654 71658 71662 71666 71694 71713 71717 71726 71731 71754 71756 71769 71786 71804 71805 71825 71829 71836 71853 71854 71865 71884 71918 71920 71933 71936 71957 71976 71988 71989 72033 72036 72037 72040 72051 72093 72117 72124 72126 72142 72153 72163 72170 72180 72186 72223 72234 72235 72243 72246 72257 72279 72283 72287 72293 72295 72297 72333 72343 72352 72400 72406 72442 72446 72451 72453 72457 72474 72478 72486 72499 72506 72511 72528 72529 72538 72539 72559 72563 72565 72569 72596 72609 72617 72626 72635 72636 72649 72660 72667 72679 72692 72696 72705 72706 72710 72712 72714 72716 72720 72722 72740 72757 72764 72768 72786 72804 72805 72811 72813 72816 72846 72861 72905 72922 72923 72937 72938 72960 72970 72987 73002 73016 73042 73056 73067 73072 73113 73119 73120 73124 73135 73138 73147 73150 73154 73162 73164 73169 73202 73217 73251 73254 73257 73262 73288 73294 73301 73317 73336 73337 73345 73352 73376 73384 73412 73415 73421 73426 73453 73472 73482 73501 73504 73506 73509 73515 73517 73525 73547 73552 73562 73572 73594 73598 73600 73609 73616 73631 73635 73650 73652 73653 73687 73690 73694 73700 73704 73707 73711 73730 73737 73738 73771 73776 73785 73787 73791 73798 73833 73835 73838 73870 73883 73892 73900 73906 73908 73913 73914 73917 73925 73930 73932 73934 73950 73951 73958 73976 73995 74019 74020 74025 74039 74040 74052 74063 74066 74076 74082 74085 74091 74093 74128 74137 74146 74149 74182 74183 74189 74223 74240 74270 74279 74285 74304 74310 74311 74316 74318 74333 74355 74358 74362 74369 74371 74375 74381 74382 74389 74393 74396 74397 74400 74411 74421 74429 74433 74436 74437 74447 74465 74475 74477 74478 74480 74482 74484 74488 74496 74501 74504 74509 74513 74529 74551 74562 74568 74587 74615 74619 74621 74630 74645 74655 74676 74701 74702 74706 74717 74723 74728 74737 74744 74763 74799 74804 74815 74828 74830 74831 74832 74840 74841 74861 74878 74884 74903 74914 74922 74924 74949 74959 74960 74964 74966 74971 74975 75015 75019 75045 75069 75087 75098 75125 75132 75149 75178 75189 75199 75203 75229 75232 75245 75261 75264 75276 75278 75281 75284 75319 75329 75335 75343 75351 75366 75370 75371 75374 75380 75382 75402 75410 75421 75431 75435 75445 75450 75480 75493 75517 75525 75528 75530 75532 75542 75556 75570 75589 75591 75601 75622 75624 75643 75649 75660 75689 75700 75707 75722 75725 75737 75742 75756 75758 75764 75776 75782 75787 75791 75808 75809 75816 75831 75844 75859 75869 75870 75880 75887 75896 75897 75898 75910 75948 75977 75997 76008 76018 76028 76036 76045 76051 76055 76059 76074 76085 76090 76138 76142 76152 76174 76175 76179 76184 76194 76208 76223 76228 76244 76259 76280 76286 76303 76313 76325 76342 76346 76356 76360 76366 76370 76375 76385 76390 76396 76400 76404 76415 76418 76422 76441 76450 76452 76475 76491 76508 76515 76531 76543 76547 76583 76588 76621 76647 76659 76677 76698 76730 76746 76748 76753 76755 76758 76774 76783 76784 76785 76787 76802 76808 76814 76833 76835 76842 76856 76865 76916 76921 76946 76949 76955 76970 76980 76983 77004 77009 77015 77018 77025 77027 77046 77048 77049 77052 77055 77061 77063 77098 77108 77119 77138 77141 77184 77190 77202 77205 77210 77216 77225 77241 77262 77264 77272 77273 77278 77289 77294 77303 77325 77328 77350 77395 77396 77414 77418 77445 77448 77473 77479 77489 77492 77510 77518 77520 77540 77560 77572 77585 77610 77615 77637 77638 77642 77646 77676 77679 77683 77687 77695 77702 77709 77720 77726 77730 77736 77743 77747 77753 77755 77757 77769 77803 77840 77855 77858 77863 77875 77889 77891 77899 77900 77906 77907 77909 77916 77923 77928 77938 77941 77943 77948 77949 77986 78002 78007 78009 78015 78027 78041 78063 78064 78074 78075 78092 78093 78097 78131 78133 78135 78136 78138 78181 78189 78191 78195 78201 78213 78225 78233 78236 78248 78262 78272 78274 78277 78278 78280 78357 78362 78388 78393 78408 78414 78415 78417 78429 78439 78442 78455 78461 78467 78487 78490 78505 78521 78525 78530 78537 78540 78546 78549 78558 78570 78571 78574 78590 78597 78610 78617 78620 78624 78626 78641 78655 78664 78665 78673 78678 78684 78694 78697 78700 78702 78710 78714 78724 78737 78758 78796 78802 78813 78822 78844 78854 78864 78865 78901 78908 78916 78917 78926 78927 78928 78934 78936 78937 78949 78965 79001 79029 79050 79059 79064 79068 79072 79076 79082 79085 79091 79098 79115 79119 79121 79122 79125 79139 79145 79147 79149 79158 79161 79206 79221 79230 79233 79244 79245 79246 79255 79257 79266 79269 79297 79305 79335 79349 79354 79356 79364 79366 79375 79381 79384 79385 79387 79415 79416 79425 79452 79479 79486 79510 79519 79534 79538 79539 79545 79563 79569 79578 79582 79588 79591 79617 79634 79635 79646 79651 79652 79659 79664 79670 79671 79687 79691 79694 79715 79729 79735 79775 79779 79798 79800 79836 79839 79845 79849 79853 79854 79855 79866 79867 79871 79893 79897 79921 79923 79928 79933 79935 79941 79967 79978 79991 79993 80007 80009 80017 80022 80053 80072 80102 80103 80118 80127 80129 80139 80142 80152 80153 80154 80174 80177 80181 80184 80203 80217 80220 80223 80233 80250 80253 80256 80257 80289 80291 80293 80302 80312 80315 80344 80396 80418 80419 80421 80428 80452 80455 80464 80481 80487 80489 80506 80513 80516 80520 80521 80527 80551 80556 80558 80574 80584 80602 80604 80605 80612 80620 80634 80635 80648 80664 80669 80671 80672 80701 80710 80725 80744 80747 80749 80763 80766 80769 80778 80792 80809 80812 80816 80826 80835 80840 80841 80860 80868 80888 80896 80905 80922 80930 80947 80956 80963 80965 80976 80984 80993 80996 81000 81007 81009 81010 81012 81028 81054 81056 81065 81071 81075 81093 81096 81115 81121 81141 81146 81171 81180 81183 81194 81197 81209 81216 81219 81222 81225 81235 81243 81246 81257 81268 81270 81285 81295 81300 81305 81311 81319 81326 81375 81387 81388 81398 81436 81466 81471 81478 81487 81494 81496 81508 81563 81571 81577 81580 81587 81605 81613 81631 81637 81649 81656 81658 81681 81685 81698 81707 81717 81744 81745 81749 81753 81755 81757 81758 81767 81769 81770 81784 81786 81795 81797 81800 81809 81824 81834 81857 81859 81881 81931 81939 81952 81960 81969 81973 81981 81982 82008 82021 82031 82038 82040 82079 82080 82094 82126 82130 82145 82153 82159 82171 82179 82181 82183 82198 82225 82236 82242 82244 82246 82248 82258 82272 82311 82344 82357 82359 82372 82373 82380 82385 82387 82395 82400 82407 82412 82441 82449 82460 82476 82478 82482 82499 82508 82516 82524 82533 82539 82549 82557 82587 82603 82607 82630 82633 82637 82643 82658 82662 82725 82734 82752 82760 82761 82771 82778 82800 82814 82818 82846 82852 82857 82858 82861 82863 82869 82880 82892 82895 82902 82913 82917 82932 82941 82942 82948 82953 82961 82966 82970 82972 82976 82985 83008 83020 83032 83033 83039 83045 83051 83055 83066 83093 83098 83101 83106 83110 83111 83120 83122 83128 83132 83133 83144 83148 83166 83167 83170 83180 83194 83207 83236 83250 83266 83280 83289 83291 83293 83302 83325 83345 83353 83379 83386 83395 83411 83428 83444 83454 83466 83475 83484 83488 83496 83498 83520 83524 83554 83557 83582 83587 83594 83604 83608 83609 83612 83613 83621 83645 83651 83673 83692 83694 83695 83696 83708 83716 83740 83753 83754 83767 83786 83790 83800 83812 83814 83823 83835 83836 83837 83839 83840 83841 83843 83876 83879 83881 83914 83932 83938 83945 83977 83983 84017 84025 84036 84053 84065 84073 84075 84091 84094 84104 84112 84124 84127 84129 84131 84152 84164 84168 84180 84186 84198 84230 84241 84267 84276 84305 84309 84332 84354 84361 84363 84374 84395 84423 84427 84441 84443 84446 84454 84456 84465 84478 84488 84489 84494 84509 84517 84521 84525 84528 84530 84531 84541 84546 84576 84581 84584 84587 84588 84589 84597 84614 84629 84648 84656 84673 84690 84691 84702 84707 84710 84720 84721 84728 84739 84746 84752 84770 84773 84782 84787 84793 84807 84815 84827 84832 84839 84847 84875 84883 84896 84919 84924 84927 84931 84938 84941 84950 84952 84964 84979 84982 84985 84991 85004 85010 85015 85023 85046 85067 85080 85086 85100 85108 85125 85130 85139 85152 85177 85186 85191 85192 85200 85202 85206 85209 85210 85219 85235 85242 85245 85273 85275 85293 85331 85354 85359 85384 85386 85397 85414 85431 85457 85461 85499 85504 85507 85516 85538 85555 85556 85575 85580 85588 85589 85632 85638 85658 85673 85680 85687 85706 85717 85728 85751 85760 85766 85768 85770 85789 85793 85794 85807 85814 85831 85854 85868 85873 85874 85894 85904 85907 85909 85915 85916 85917 85919 85928 85930 85931 85950 85971 85973 85974 85981 85988 86034 86036 86038 86041 86044 86047 86051 86055 86060 86070 86072 86090 86093 86097 86101 86117 86137 86148 86158 86172 86210 86250 86254 86257 86260 86263 86279 86295 86297 86305 86335 86343 86362 86366 86379 86382 86384 86394 86401 86402 86420 86444 86463 86465 86467 86470 86483 86490 86506 86523 86526 86533 86534 86539 86554 86573 86575 86585 86614 86634 86635 86645 86661 86664 86679 86688 86709 86710 86712 86730 86742 86743 86759 86763 86765 86770 86801 86807 86827 86830 86847 86872 86880 86888 86892 86894 86905 86906 86908 86911 86913 86921 86929 86934 86943 86959 86960 86962 86964 86970 86971 86989 86994 87008 87032 87033 87042 87043 87046 87048 87053 87057 87070 87072 87077 87098 87121 87128 87134 87135 87142 87146 87157 87165 87169 87176 87183 87197 87203 87205 87220 87230 87235 87253 87284 87290 87300 87303 87310 87343 87347 87352 87354 87357 87359 87377 87383 87385 87387 87388 87391 87398 87400 87403 87404 87408 87423 87424 87428 87477 87502 87506 87510 87514 87515 87527 87537 87538 87539 87547 87552 87562 87568 87573 87575 87625 87664 87669 87694 87707 87718 87740 87752 87777 87782 87793 87794 87825 87826 87838 87840 87845 87848 87857 87860 87870 87871 87883 87895 87907 87911 87919 87923 87930 87953 87964 87977 87979 87987 87994 87999 88000 88014 88021 88035 88058 88061 88083 88094 88100 88105 88109 88110 88118 88119 88134 88135 88143 88144 88155 88162 88181 88199 88207 88224 88225 88235 88236 88243 88246 88251 88253 88275 88293 88300 88327 88337 88371 88373 88375 88417 88472 88474 88488 88496 88504 88505 88507 88508 88521 88528 88537 88542 88591 88605 88610 88619 88670 88680 88683 88701 88717 88723 88724 88735 88744 88745 88750 88766 88777 88779 88784 88788 88797 88801 88820 88822 88828 88838 88865 88883 88893 88897 88908 88925 88931 88942 88953 88955 88961 88966 88976 89011 89018 89027 89036 89052 89064 89076 89091 89121 89131 89135 89136 89141 89145 89146 89158 89179 89181 89189 89194 89207 89227 89228 89236 89237 89240 89261 89284 89285 89291 89294 89300 89323 89326 89329 89330 89338 89366 89370 89373 89376 89381 89383 89390 89393 89394 89398 89410 89412 89424 89426 89456 89461 89472 89490 89499 89502 89509 89513 89534 89535 89543 89544 89550 89568 89572 89577 89583 89585 89594 89597 89608 89613 89615 89619 89633 89645 89653 89654 89676 89681 89682 89713 89720 89727 89745 89752 89786 89790 89792 89793 89805 89821 89826 89846 89863 89872 89884 89900 89904 89908 89924 89929 89932 89939 89943 89953 89966 89968 89970 89977 89980 89982 89997 90006 90016 90025 90028 90034 90038 90046 90049 90087 90091 90100 90127 90154 90161 90170 90177 90178 90179 90192 90196 90201 90219 90235 90280 90284 90306 90307 90312 90314 90327 90330 90357 90363 90368 90394 90425 90429 90437 90458 90489 90490 90499 90503 90520 90534 90553 90558 90579 90581 90585 90592 90610 90613 90618 90628 90654 90700 90722 90724 90728 90731 90732 90740 90748 90750 90761 90788 90802 90809 90814 90833 90837 90838 90853 90878 90881 90884 90889 90895 90905 90911 90935 90941 90957 90961 90962 90985 90990 90993 90994 90995 90999 91030 91044 91046 91048 91054 91080 91097 91114 91128 91132 91142 91149 91159 91161 91170 91177 91178 91194 91200 91203 91233 91281 91284 91289 91295 91300 91302 91303 91309 91310 91316 91318 91322 91374 91377 91389 91408 91413 91425 91431 91439 91453 91491 91498 91527 91528 91531 91538 91539 91540 91548 91557 91576 91577 91579 91591 91599 91603 91610 91612 91624 91645 91648 91651 91660 91682 91690 91709 91728 91733 91748 91777 91782 91791 91815 91818 91830 91832 91854 91856 91857 91859 91867 91911 91933 91939 91947 91949 91950 91953 91961 91965 91979 92014 92025 92028 92040 92060 92062 92087 92107 92115 92129 92147 92148 92157 92178 92180 92182 92185 92193 92194 92198 92203 92257 92262 92281 92288 92326 92332 92333 92364 92384 92411 92424 92427 92431 92447 92460 92473 92480 92488 92517 92528 92532 92571 92584 92588 92592 92598 92613 92615 92616 92629 92635 92642 92648 92655 92681 92705 92708 92725 92744 92748 92762 92789 92824 92840 92848 92853 92858 92860 92863 92866 92879 92885 92905 92908 92915 92916 92944 92972 92975 92986 92996 93006 93012 93013 93039 93041 93046 93057 93068 93070 93078 93093 93102 93107 93110 93120 93158 93160 93175 93182 93196 93216 93219 93224 93242 93252 93285 93290 93313 93321 93342 93355 93359 93367 93378 93380 93385 93386 93393 93409 93431 93432 93440 93445 93457 93486 93488 93498 93505 93512 93513 93518 93519 93527 93535 93547 93568 93569 93576 93584 93585 93589 93594 93600 93615 93620 93621 93624 93631 93638 93648 93649 93655 93658 93660 93662 93672 93677 93681 93687 93692 93696 93699 93703 93706 93710 93731 93734 93742 93751 93769 93788 93790 93808 93809 93810 93848 93858 93866 93888 93893 93898 93900 93903 93909 93912 93920 93927 93928 93934 93972 93989 94012 94034 94035 94049 94051 94055 94057 94065 94069 94075 94079 94104 94111 94139 94159 94162 94166 94189 94194 94197 94206 94215 94228 94240 94248 94255 94258 94268 94269 94276 94283 94292 94298 94336 94343 94354 94380 94398 94418 94422 94447 94451 94454 94466 94467 94472 94504 94505 94506 94515 94517 94518 94527 94531 94537 94547 94556 94561 94570 94587 94604 94607 94613 94617 94625 94635 94639 94644 94649 94678 94682 94692 94711 94737 94740 94763 94859 94864 94868 94876 94886 94898 94899 94912 94914 94923 94924 94929 94930 94940 94942 94951 94965 94986 95001 95012 95034 95035 95036 95051 95062 95078 95086 95096 95105 95112 95126 95136 95154 95179 95191 95196 95219 95235 95236 95243 95249 95265 95274 95279 95282 95283 95287 95312 95324 95333 95351 95381 95385 95386 95390 95398 95400 95406 95408 95415 95423 95427 95434 95448 95454 95465 95467 95472 95476 95480 95498 95520 95521 95542 95557 95565 95580 95602 95622 95632 95640 95644 95652 95653 95662 95682 95685 95688 95691 95698 95702 95715 95718 95742 95746 95757 95761 95775 95784 95805 95808 95819 95826 95846 95861 95878 95925 95936 95944 95947 95958 95960 95968 95971 95987 95999 96001 96005 96038 96039 96055 96066 96079 96097 96104 96105 96108 96120 96178 96184 96226 96230 96238 96247 96248 96252 96267 96276 96289 96297 96325 96341 96347 96349 96352 96353 96354 96367 96368 96382 96407 96414 96415 96425 96426 96428 96432 96457 96463 96466 96470 96485 96499 96510 96527 96549 96555 96556 96588 96598 96619 96627 96644 96648 96663 96679 96680 96689 96700 96710 96740 96749 96751 96763 96771 96774 96780 96782 96802 96805 96826 96837 96838 96842 96843 96852 96854 96857 96859 96875 96879 96885 96887 96898 96904 96911 96919 96952 96953 96989 97015 97017 97027 97035 97041 97047 97074 97097 97131 97133 97137 97141 97144 97146 97151 97187 97221 97224 97228 97234 97238 97243 97257 97262 97271 97278 97280 97298 97304 97309 97311 97322 97325 97327 97332 97337 97339 97357 97371 97383 97392 97399 97407 97409 97425 97430 97454 97467 97479 97490 97511 97530 97533 97548 97554 97564 97567 97569 97669 97674 97676 97686 97693 97696 97698 97702 97711 97721 97731 97756 97758 97759 97767 97780 97825 97834 97844 97851 97862 97867 97878 97885 97886 97889 97890 97903 97921 97926 97927 97932 97939 97952 97970 97996 97998 98004 98012 98013 98031 98032 98049 98051 98058 98067 98068 98074 98095 98104 98106 98122 98126 98134 98140 98161 98165 98172 98181 98185 98191 98208 98219 98221 98222 98231 98253 98256 98258 98259 98263 98265 98269 98291 98300 98302 98308 98329 98342 98350 98380 98387 98396 98402 98408 98428 98444 98448 98458 98469 98470 98472 98494 98524 98543 98549 98574 98582 98583 98587 98593 98623 98631 98647 98650 98663 98678 98681 98686 98691 98712 98722 98725 98729 98731 98732 98734 98761 98783 98785 98804 98818 98822 98847 98869 98875 98890 98899 98906 98909 98927 98928 98935 98976 98994 99003 99026 99034 99036 99054 99065 99068 99069 99075 99080 99085 99093 99096 99113 99126 99146 99154 99157 99164 99176 99177 99180 99182 99193 99238 99240 99241 99246 99286 99288 99305 99317 99320 99331 99353 99356 99365 99410 99412 99419 99434 99440 99444 99447 99448 99453 99465 99471 99477 99506 99508 99534 99539 99563 99587 99597 99599 99608 99613 99633 99668 99706 99718 99722 99723 99736 99749 99751 99766 99771 99774 99776 99784 99801 99813 99819 99828 99830 99849 99852 99854 99888 99899 99914 99924 99929 99941 99977 99980 99987 21 34 36 50 61 71 79 99 104 108 109 113 120 128 130 132 138 144 149 164 171 181 184 188 205 211 214 217 221 222 236 241 263 279 280 282 284 293 305 314 329 333 343 345 346 354 356 371 409 411 413 418 422 433 449 456 458 499 504 509 531 545 562 568 578 587 598 634 656 677 683 703 704 716 723 727 729 730 751 758 773 791 802 812 815 829 835 839 840 869 878 890 894 915 928 930 932 939 940 945 957 961 971 972 981 985 1003 1006 1007 1036 1043 1049 1053 1061 1065 1080 1091 1096 1097 1101 1109 1119 1123 1133 1137 1148 1150 1159 1161 1177 1191 1192 1203 1210 1223 1230 1237 1241 1256 1259 1260 1264 1270 1283 1293 1299 1344 1350 1356 1375 1376 1389 1411 1434 1435 1437 1438 1449 1454 1459 1464 1475 1512 1513 1537 1541 1543 1563 1568 1600 1608 1610 1619 1621 1630 1648 1655 1668 1671 1691 1698 1738 1740 1742 1756 1761 1764 1766 1783 1790 1807 1813 1815 1821 1824 1826 1831 1838 1842 1845 1851 1853 1881 1892 1910 1921 1928 1938 1940 1967 1975 1982 1990 1995 2014 2016 2035 2054 2059 2075 2097 2106 2124 2125 2137 2140 2143 2146 2163 2182 2209 2211 2212 2227 2228 2232 2234 2235 2238 2243 2256 2261 2263 2265 2267 2270 2283 2294 2298 2306 2319 2323 2330 2332 2340 2351 2365 2370 2404 2409 2420 2440 2459 2464 2473 2476 2484 2494 2495 2514 2536 2539 2549 2562 2579 2583 2584 2591 2607 2616 2627 2663 2666 2678 2687 2690 2703 2715 2719 2731 2766 2777 2794 2797 2798 2805 2820 2830 2836 2843 2845 2849 2851 2862 2868 2869 2879 2880 2881 2887 2897 2916 2926 2928 2934 2942 2954 2961 2967 2989 2993 2996 3001 3020 3045 3051 3063 3064 3073 3074 3115 3136 3144 3145 3146 3152 3155 3167 3196 3208 3209 3231 3242 3261 3269 3284 3295 3322 3334 3354 3372 3376 3391 3396 3402 3419 3425 3434 3440 3441 3442 3451 3477 3489 3493 3505 3508 3512 3547 3550 3560 3567 3577 3584 3591 3595 3615 3616 3621 3625 3627 3670 3680 3694 3701 3744 3779 3784 3792 3812 3817 3821 3825 3827 3834 3845 3854 3856 3869 3884 3885 3888 3890 3891 3899 3906 3910 3915 3924 3925 3943 3945 3951 3976 4000 4009 4010 4013 4027 4029 4063 4065 4074 4080 4081 4084 4091 4097 4107 4118 4125 4141 4144 4158 4160 4163 4172 4184 4186 4209 4213 4214 4233 4234 4242 4245 4248 4259 4260 4262 4264 4273 4279 4293 4312 4318 4321 4326 4338 4346 4364 4369 4379 4393 4395 4401 4412 4446 4450 4474 4507 4515 4521 4558 4561 4566 4573 4574 4586 4589 4593 4596 4619 4634 4642 4650 4654 4657 4658 4661 4678 4688 4694 4697 4705 4714 4721 4723 4728 4735 4742 4773 4801 4803 4809 4826 4842 4843 4850 4851 4854 4866 4867 4872 4882 4885 4895 4899 4932 4955 4977 4983 4989 4999 5006 5011 5020 5031 5055 5057 5058 5061 5070 5080 5106 5109 5118 5136 5149 5168 5171 5188 5191 5194 5195 5198 5206 5226 5231 5233 5242 5272 5286 5296 5300 5325 5332 5338 5340 5360 5364 5367 5380 5387 5389 5390 5397 5408 5415 5450 5456 5459 5467 5484 5491 5515 5516 5523 5528 5538 5556 5580 5584 5585 5586 5595 5600 5607 5625 5629 5648 5656 5661 5667 5680 5686 5688 5703 5742 5757 5792 5831 5849 5866 5874 5879 5891 5909 5916 5930 5951 5954 5970 5976 5993 5997 6009 6011 6018 6045 6055 6063 6066 6073 6125 6132 6153 6182 6183 6202 6203 6209 6215 6227 6233 6248 6249 6266 6285 6289 6295 6307 6309 6318 6324 6333 6334 6338 6342 6345 6350 6358 6364 6378 6382 6396 6431 6435 6436 6459 6478 6524 6532 6589 6592 6609 6611 6623 6632 6647 6649 6669 6674 6684 6689 6706 6720 6736 6755 6774 6777 6783 6784 6786 6788 6799 6814 6823 6834 6839 6843 6849 6863 6870 6878 6882 6908 6914 6917 6918 6931 6953 6954 6955 6968 6980 6983 6994 6995 6997 6999 7009 7011 7032 7033 7036 7058 7059 7084 7101 7114 7119 7139 7146 7149 7164 7165 7183 7186 7196 7200 7217 7239 7253 7258 7283 7296 7298 7322 7328 7346 7350 7354 7356 7361 7391 7402 7403 7427 7443 7460 7466 7467 7513 7516 7519 7543 7544 7546 7547 7572 7582 7590 7619 7621 7644 7693 7695 7702 7707 7710 7713 7714 7738 7754 7758 7776 7786 7796 7797 7798 7811 7812 7813 7815 7825 7869 7878 7881 7895 7898 7908 7931 7937 7940 7979 7982 7988 7990 7999 8001 8004 8010 8014 8018 8030 8057 8075 8082 8085 8098 8100 8119 8130 8131 8137 8187 8193 8201 8209 8238 8256 8278 8290 8293 8296 8297 8302 8303 8308 8314 8315 8321 8322 8335 8350 8361 8368 8375 8376 8389 8391 8400 8402 8432 8433 8444 8448 8456 8465 8468 8470 8473 8495 8510 8512 8513 8524 8533 8547 8577 8588 8598 8599 8605 8612 8626 8641 8648 8655 8669 8696 8702 8704 8706 8724 8733 8757 8766 8781 8782 8792 8841 8845 8849 8856 8858 8862 8863 8876 8877 8884 8898 8900 8905 8953 8956 8958 8964 8975 8978 8983 8989 8992 8994 9000 9013 9031 9049 9055 9071 9076 9091 9111 9113 9132 9139 9147 9153 9175 9198 9202 9209 9215 9216 9219 9222 9225 9229 9231 9239 9245 9257 9264 9273 9291 9308 9318 9324 9345 9368 9370 9373 9386 9388 9394 9399 9407 9435 9449 9477 9479 9487 9526 9528 9529 9553 9567 9574 9576 9577 9583 9592 9623 9628 9638 9644 9645 9658 9665 9701 9708 9713 9715 9716 9718 9722 9738 9741 9771 9776 9784 9790 9793 9808 9810 9823 9829 9830 9842 9844 9848 9849 9857 9861 9882 9904 9910 9932 9946 9951 9965 9984 9988 9992 9995 10003 10013 10030 10038 10068 10104 10110 10124 10136 10139 10142 10144 10154 10171 10173 10182 10186 10197 10198 10202 10206 10217 10228 10230 10231 10238 10247 10251 10278 10303 10311 10318 10321 10324 10326 10353 10375 10376 10379 10380 10384 10401 10423 10424 10430 10442 10449 10453 10473 10475 10476 10481 10516 10571 10575 10579 10585 10602 10614 10618 10627 10636 10639 10686 10690 10693 10709 10722 10758 10770 10771 10777 10781 10783 10786 10806 10810 10823 10849 10853 10855 10860 10870 10871 10883 10887 10895 10899 10917 10919 10928 10932 10941 10951 10960 10967 10976 10986 10994 11014 11016 11026 11029 11052 11054 11068 11077 11089 11094 11115 11117 11139 11143 11148 11154 11159 11163 11171 11180 11201 11208 11231 11237 11269 11290 11294 11298 11308 11316 11323 11336 11338 11346 11347 11350 11364 11366 11378 11380 11419 11420 11426 11438 11439 11450 11469 11471 11482 11521 11559 11567 11571 11596 11601 11606 11608 11621 11624 11626 11632 11646 11651 11652 11654 11656 11658 11664 11666 11677 11679 11680 11684 11685 11735 11740 11743 11755 11783 11787 11796 11802 11808 11821 11822 11829 11838 11846 11851 11886 11887 11891 11903 11907 11931 11946 11962 11964 12002 12039 12043 12045 12046 12055 12056 12067 12079 12082 12095 12105 12121 12137 12143 12146 12147 12164 12171 12177 12189 12193 12202 12203 12212 12227 12231 12240 12254 12271 12272 12288 12289 12292 12304 12310 12315 12321 12329 12334 12336 12363 12366 12402 12411 12417 12418 12419 12431 12435 12442 12458 12464 12472 12481 12485 12501 12505 12508 12525 12553 12573 12576 12578 12583 12607 12621 12634 12636 12639 12640 12646 12649 12665 12667 12686 12691 12697 12704 12725 12730 12732 12733 12738 12755 12777 12803 12817 12832 12844 12845 12848 12856 12859 12868 12874 12876 12886 12888 12896 12911 12915 12918 12938 12956 12957 12973 12982 13027 13030 13032 13040 13050 13066 13110 13113 13116 13122 13126 13129 13141 13171 13176 13199 13220 13226 13228 13267 13278 13294 13295 13310 13316 13345 13348 13381 13401 13402 13407 13417 13427 13428 13460 13463 13466 13467 13482 13496 13522 13523 13525 13528 13536 13555 13558 13573 13619 13653 13655 13662 13663 13669 13672 13673 13725 13727 13733 13753 13755 13760 13764 13766 13777 13794 13801 13805 13808 13813 13814 13841 13843 13857 13858 13870 13879 13890 13910 13913 13920 13922 13960 13976 13982 13991 13998 14019 14020 14021 14023 14028 14044 14075 14088 14090 14094 14102 14105 14113 14121 14126 14133 14142 14143 14147 14150 14162 14180 14193 14207 14211 14227 14247 14248 14254 14256 14257 14276 14282 14288 14303 14318 14326 14337 14338 14346 14362 14368 14378 14381 14405 14418 14421 14426 14440 14441 14443 14458 14478 14480 14485 14487 14499 14510 14514 14518 14537 14555 14556 14559 14572 14592 14600 14621 14629 14640 14660 14661 14663 14672 14677 14679 14696 14699 14702 14703 14706 14714 14717 14727 14729 14750 14757 14776 14822 14827 14830 14856 14872 14879 14883 14885 14888 14904 14931 14942 14944 14957 14963 14993 15011 15018 15019 15033 15066 15077 15078 15110 15130 15151 15155 15176 15195 15204 15209 15219 15242 15269 15290 15305 15309 15315 15348 15352 15363 15370 15371 15377 15378 15387 15399 15406 15421 15424 15426 15433 15436 15439 15442 15451 15469 15480 15481 15485 15489 15490 15505 15508 15514 15521 15522 15533 15552 15554 15560 15565 15571 15572 15574 15576 15577 15619 15622 15664 15676 15697 15723 15731 15754 15762 15806 15813 15821 15837 15847 15849 15863 15868 15886 15918 15922 15925 15928 15958 15970 15976 15984 15988 15989 16021 16030 16034 16039 16051 16080 16086 16089 16091 16100 16131 16140 16144 16153 16168 16226 16228 16233 16234 16244 16256 16279 16285 16287 16302 16303 16318 16329 16332 16341 16347 16359 16374 16383 16384 16389 16393 16404 16408 16414 16430 16449 16469 16476 16478 16484 16491 16498 16527 16533 16564 16575 16580 16582 16593 16604 16611 16618 16620 16624 16634 16637 16643 16645 16653 16658 16662 16678 16691 16693 16701 16707 16709 16715 16746 16755 16845 16848 16888 16899 16918 16950 16957 16969 16978 16983 16993 17000 17009 17021 17036 17063 17073 17090 17100 17118 17126 17129 17133 17157 17159 17166 17173 17176 17177 17195 17198 17204 17210 17211 17222 17231 17236 17255 17280 17284 17288 17290 17304 17315 17324 17330 17336 17364 17369 17371 17377 17380 17387 17391 17400 17413 17418 17422 17423 17427 17431 17448 17449 17473 17484 17511 17538 17551 17553 17561 17568 17578 17583 17586 17614 17625 17633 17638 17639 17640 17647 17660 17670 17672 17673 17675 17682 17693 17707 17716 17717 17722 17737 17754 17758 17770 17832 17834 17854 17875 17879 17908 17909 17912 17928 17930 17944 17946 17950 17961 17970 17974 17976 17982 17985 17986 17992 17996 17998 17999 18004 18010 18012 18016 18023 18036 18040 18044 18045 18050 18060 18067 18079 18084 18117 18135 18155 18156 18166 18181 18196 18230 18231 18235 18242 18243 18254 18271 18273 18275 18280 18307 18310 18311 18321 18329 18334 18335 18350 18356 18363 18386 18395 18400 18407 18413 18421 18422 18430 18440 18453 18455 18461 18484 18487 18497 18509 18510 18522 18528 18537 18550 18553 18555 18575 18579 18588 18594 18604 18606 18621 18634 18640 18672 18714 18721 18726 18732 18734 18744 18745 18746 18747 18758 18763 18764 18777 18782 18823 18830 18873 18890 18938 18959 18969 18981 18984 18985 18995 18999 19000 19008 19011 19020 19037 19041 19051 19056 19107 19112 19120 19122 19124 19126 19143 19147 19148 19158 19164 19176 19178 19180 19183 19184 19186 19198 19202 19205 19208 19210 19225 19231 19232 19259 19265 19266 19285 19286 19306 19326 19338 19344 19356 19363 19366 19372 19383 19387 19390 19398 19410 19421 19443 19446 19449 19456 19467 19485 19486 19488 19498 19499 19502 19507 19532 19551 19555 19570 19572 19577 19633 19646 19653 19691 19703 19705 19724 19725 19744 19760 19773 19779 19786 19793 19794 19795 19803 19806 19809 19813 19839 19850 19857 19869 19874 19902 19908 19914 19926 19936 19945 19955 19976 19994 20004 20006 20020 20034 20041 20047 20050 20060 20073 20081 20087 20115 20130 20143 20152 20167 20176 20183 20187 20196 20210 20223 20231 20233 20245 20264 20265 20287 20304 20320 20337 20346 20350 20360 20361 20391 20399 20414 20419 20420 20421 20432 20433 20443 20455 20460 20467 20471 20473 20488 20490 20503 20505 20524 20526 20543 20544 20570 20579 20587 20598 20608 20612 20613 20622 20635 20640 20642 20648 20651 20673 20676 20679 20681 20684 20685 20727 20744 20760 20769 20782 20790 20796 20818 20826 20827 20828 20834 20837 20843 20846 20863 20867 20879 20884 20900 20912 20914 20919 20928 20929 20938 20952 20957 20970 20988 20992 20998 20999 21020 21033 21044 21051 21053 21059 21064 21072 21073 21092 21099 21102 21109 21112 21125 21126 21135 21140 21169 21171 21177 21206 21208 21224 21227 21232 21238 21242 21253 21262 21281 21297 21307 21316 21348 21353 21379 21380 21382 21394 21399 21405 21408 21409 21410 21426 21450 21454 21455 21462 21469 21477 21482 21500 21506 21517 21523 21533 21542 21544 21549 21565 21566 21568 21571 21573 21584 21586 21589 21604 21608 21611 21624 21645 21647 21665 21666 21683 21740 21743 21745 21753 21761 21764 21768 21772 21783 21795 21798 21799 21805 21845 21850 21865 21886 21899 21912 21928 21936 21939 21954 21962 21983 21986 21992 21999 22001 22040 22056 22063 22108 22144 22171 22187 22205 22207 22231 22271 22274 22292 22301 22310 22319 22340 22365 22366 22371 22378 22379 22388 22399 22401 22405 22421 22424 22427 22429 22442 22450 22452 22460 22469 22470 22472 22478 22484 22490 22495 22497 22502 22509 22512 22517 22547 22548 22549 22551 22556 22557 22567 22576 22579 22591 22592 22595 22599 22603 22606 22610 22628 22665 22669 22679 22691 22693 22698 22711 22736 22744 22745 22774 22780 22789 22811 22813 22821 22840 22844 22852 22859 22868 22873 22880 22891 22900 22902 22914 22932 22948 22949 22952 22999 23013 23034 23050 23052 23059 23067 23068 23093 23114 23121 23132 23136 23142 23160 23170 23176 23181 23182 23194 23208 23211 23224 23225 23243 23270 23301 23303 23317 23320 23325 23339 23341 23344 23354 23366 23379 23387 23403 23409 23415 23416 23417 23438 23441 23448 23468 23475 23482 23485 23493 23513 23535 23537 23539 23542 23560 23564 23579 23587 23592 23602 23606 23613 23619 23625 23643 23644 23646 23658 23664 23667 23673 23675 23683 23688 23699 23709 23717 23734 23741 23744 23750 23762 23784 23785 23788 23806 23817 23826 23831 23833 23834 23835 23836 23840 23852 23856 23858 23872 23877 23884 23900 23903 23911 23915 23934 23980 23994 23995 24002 24010 24034 24040 24054 24061 24065 24068 24069 24070 24076 24101 24118 24123 24128 24129 24142 24147 24160 24161 24162 24164 24167 24178 24181 24188 24191 24194 24219 24225 24226 24246 24261 24290 24315 24319 24321 24324 24350 24373 24394 24396 24407 24414 24447 24448 24452 24471 24480 24487 24500 24512 24516 24519 24535 24537 24541 24544 24551 24590 24596 24599 24655 24660 24661 24687 24694 24702 24707 24724 24741 24742 24757 24779 24782 24788 24810 24817 24818 24821 24828 24839 24840 24851 24854 24858 24861 24895 24898 24905 24906 24911 24922 24930 24940 24947 24956 24966 24988 24989 24991 24996 25017 25027 25034 25038 25043 25044 25050 25052 25054 25056 25077 25080 25084 25087 25100 25104 25109 25128 25141 25153 25164 25189 25201 25211 25218 25231 25235 25238 25251 25266 25271 25274 25275 25282 25286 25288 25290 25293 25313 25344 25360 25373 25376 25383 25385 25402 25410 25414 25415 25431 25437 25441 25464 25480 25482 25485 25500 25506 25507 25514 25523 25535 25538 25546 25567 25571 25578 25583 25599 25632 25643 25645 25663 25675 25678 25685 25687 25693 25716 25723 25737 25742 25761 25770 25775 25796 25797 25815 25823 25844 25853 25855 25879 25884 25891 25898 25909 25935 25942 25944 25955 25957 25961 25967 25988 25995 26007 26010 26029 26030 26031 26046 26053 26057 26062 26064 26066 26086 26089 26101 26122 26129 26130 26134 26154 26156 26175 26212 26221 26224 26230 26232 26246 26268 26269 26274 26307 26313 26323 26334 26355 26360 26381 26382 26412 26415 26431 26434 26435 26441 26445 26448 26453 26458 26479 26491 26510 26519 26541 26545 26563 26564 26594 26597 26600 26602 26626 26647 26652 26656 26660 26683 26691 26692 26695 26721 26729 26748 26754 26766 26771 26773 26798 26805 26806 26817 26839 26841 26847 26849 26850 26880 26892 26906 26915 26930 26933 26938 26942 26946 26948 26969 26971 26974 26981 26985 27007 27030 27045 27066 27074 27085 27139 27140 27143 27147 27186 27188 27202 27227 27237 27238 27250 27264 27266 27280 27283 27290 27304 27306 27314 27329 27335 27337 27356 27372 27411 27412 27419 27420 27428 27443 27450 27454 27464 27473 27480 27492 27497 27500 27508 27512 27514 27515 27519 27522 27524 27540 27551 27556 27557 27558 27574 27580 27595 27601 27605 27612 27615 27626 27633 27649 27675 27677 27678 27680 27687 27716 27733 27756 27781 27783 27809 27818 27822 27842 27846 27848 27861 27885 27886 27891 27898 27904 27924 27978 27984 27991 28003 28006 28008 28013 28037 28039 28046 28064 28071 28088 28091 28105 28116 28119 28120 28129 28138 28164 28175 28184 28199 28201 28207 28209 28225 28269 28281 28296 28298 28314 28333 28349 28373 28380 28381 28388 28404 28409 28417 28419 28427 28463 28464 28466 28467 28469 28472 28492 28499 28524 28525 28528 28546 28554 28580 28590 28595 28602 28607 28620 28641 28648 28649 28677 28683 28685 28691 28697 28710 28712 28728 28733 28741 28748 28767 28812 28839 28852 28855 28857 28860 28869 28875 28886 28891 28909 28918 28930 28958 28967 28973 28974 28977 28978 28980 28986 29006 29010 29016 29037 29043 29093 29096 29104 29112 29113 29115 29119 29147 29156 29184 29212 29230 29242 29256 29260 29261 29266 29277 29286 29289 29298 29303 29322 29325 29347 29350 29381 29401 29415 29418 29421 29434 29436 29446 29447 29451 29463 29465 29467 29475 29478 29488 29494 29502 29508 29537 29539 29560 29561 29573 29578 29589 29597 29608 29613 29616 29641 29643 29653 29699 29705 29715 29722 29724 29765 29768 29770 29786 29787 29803 29822 29824 29827 29847 29854 29856 29864 29869 29889 29896 29909 29930 29932 29933 29941 29943 29955 29961 29970 29980 29991 29994 30004 30013 30056 30072 30074 30086 30089 30091 30094 30115 30118 30132 30137 30139 30161 30165 30177 30189 30195 30227 30239 30265 30275 30294 30297 30306 30307 30318 30319 30329 30332 30346 30358 30364 30374 30377 30382 30390 30420 30421 30428 30436 30445 30449 30467 30469 30473 30478 30514 30544 30546 30550 30556 30571 30581 30585 30586 30588 30598 30599 30607 30613 30622 30624 30626 30638 30646 30690 30701 30708 30710 30716 30742 30771 30779 30790 30807 30814 30815 30817 30856 30871 30874 30889 30894 30902 30917 30919 30948 30956 30957 30958 30966 30971 30973 30974 30978 30984 30985 30988 30991 30993 31001 31013 31023 31024 31025 31034 31042 31067 31068 31074 31103 31108 31126 31132 31134 31181 31194 31197 31208 31209 31225 31243 31245 31247 31263 31272 31277 31280 31285 31322 31329 31338 31339 31348 31349 31353 31355 31370 31371 31375 31380 31410 31411 31424 31427 31429 31432 31450 31454 31464 31487 31488 31505 31508 31519 31520 31521 31531 31534 31537 31550 31561 31577 31586 31590 31594 31596 31599 31606 31615 31627 31630 31648 31649 31651 31661 31692 31699 31721 31730 31732 31736 31739 31742 31744 31761 31773 31795 31816 31817 31820 31849 31869 31872 31877 31885 31887 31893 31904 31912 31929 31930 31936 31937 31943 31951 31958 31964 31969 31978 31979 31990 31993 32007 32014 32017 32022 32042 32046 32072 32075 32081 32084 32088 32110 32119 32126 32139 32147 32150 32155 32172 32196 32208 32212 32213 32214 32217 32230 32252 32255 32258 32280 32289 32291 32296 32310 32343 32344 32352 32364 32368 32375 32398 32414 32444 32446 32453 32462 32481 32496 32506 32524 32533 32535 32543 32566 32581 32588 32590 32617 32623 32630 32639 32642 32647 32651 32660 32668 32673 32694 32708 32715 32716 32729 32736 32737 32751 32760 32763 32785 32814 32820 32824 32825 32830 32833 32837 32838 32858 32866 32867 32868 32872 32877 32885 32888 32895 32902 32905 32917 32920 32936 32940 32956 32957 32961 32985 32988 33006 33013 33014 33027 33037 33045 33050 33059 33062 33064 33079 33092 33102 33113 33116 33117 33118 33122 33136 33144 33145 33149 33150 33155 33165 33172 33192 33201 33225 33249 33253 33277 33278 33291 33293 33306 33309 33311 33317 33332 33340 33351 33377 33378 33379 33392 33393 33417 33448 33461 33462 33468 33471 33475 33500 33507 33509 33521 33530 33539 33543 33546 33569 33591 33600 33604 33636 33638 33651 33664 33698 33707 33726 33727 33738 33747 33749 33751 33767 33776 33803 33810 33814 33824 33852 33861 33881 33887 33891 33902 33905 33910 33913 33919 33928 33929 33939 33946 33955 33957 33958 33970 33975 33979 33986 33993 34008 34039 34053 34063 34074 34080 34086 34098 34102 34124 34131 34139 34142 34158 34162 34181 34182 34185 34187 34194 34202 34211 34212 34216 34230 34231 34235 34238 34273 34277 34291 34303 34327 34347 34368 34414 34426 34429 34433 34436 34437 34463 34474 34482 34485 34488 34490 34497 34519 34545 34548 34570 34576 34578 34585 34590 34627 34638 34639 34658 34664 34665 34670 34678 34693 34695 34727 34735 34747 34748 34750 34758 34781 34790 34802 34814 34823 34840 34848 34856 34859 34865 34868 34869 34870 34878 34896 34907 34913 34914 34924 34934 34938 34940 34943 34950 34951 34955 34959 34993 34996 35011 35017 35020 35021 35024 35044 35054 35062 35064 35078 35082 35087 35123 35126 35128 35136 35142 35163 35165 35166 35169 35170 35178 35194 35202 35204 35216 35229 35245 35253 35258 35260 35267 35269 35270 35273 35285 35296 35298 35303 35304 35305 35328 35334 35350 35385 35404 35410 35452 35453 35464 35466 35473 35478 35487 35491 35500 35512 35515 35526 35542 35561 35562 35570 35580 35589 35605 35625 35632 35640 35644 35647 35666 35667 35672 35697 35706 35714 35715 35726 35755 35762 35767 35769 35787 35803 35829 35836 35849 35852 35854 35856 35858 35877 35890 35897 35899 35902 35908 35909 35915 35916 35923 35930 35933 35940 35945 35946 35949 35951 35961 35971 35972 35976 35980 35991 36001 36003 36009 36020 36025 36036 36046 36070 36074 36078 36098 36102 36106 36108 36143 36148 36150 36164 36186 36188 36204 36227 36230 36249 36250 36256 36262 36289 36295 36303 36307 36308 36309 36322 36326 36363 36378 36393 36399 36400 36427 36431 36451 36456 36457 36472 36491 36492 36501 36511 36517 36532 36538 36545 36550 36552 36560 36573 36581 36583 36585 36606 36612 36626 36636 36640 36646 36647 36648 36656 36669 36670 36690 36705 36706 36714 36719 36724 36730 36737 36743 36752 36757 36773 36782 36807 36809 36811 36814 36819 36822 36825 36832 36839 36849 36851 36868 36872 36882 36889 36894 36896 36900 36908 36911 36914 36923 36927 36928 36942 36946 36973 36982 37005 37008 37009 37019 37032 37039 37042 37051 37057 37066 37077 37087 37088 37095 37103 37107 37117 37128 37144 37145 37157 37161 37163 37175 37176 37180 37181 37207 37217 37222 37235 37246 37270 37275 37281 37288 37305 37321 37350 37355 37359 37361 37365 37367 37369 37375 37403 37414 37418 37421 37448 37452 37484 37492 37496 37520 37523 37536 37539 37562 37567 37571 37574 37578 37595 37601 37610 37620 37631 37659 37670 37698 37708 37713 37724 37730 37733 37736 37748 37770 37779 37781 37817 37820 37839 37847 37876 37882 37885 37898 37901 37925 37931 37951 37973 37983 37988 37989 38001 38030 38042 38065 38093 38107 38122 38130 38135 38142 38143 38153 38161 38169 38174 38186 38199 38203 38221 38227 38231 38257 38258 38282 38284 38288 38296 38318 38324 38334 38348 38363 38375 38398 38415 38439 38461 38464 38468 38525 38527 38528 38536 38543 38548 38554 38601 38645 38657 38662 38664 38678 38685 38693 38700 38703 38711 38712 38726 38729 38732 38740 38746 38751 38767 38770 38785 38793 38807 38817 38822 38824 38831 38837 38842 38848 38860 38868 38896 38911 38920 38952 38956 38963 38964 38966 38969 38982 38984 38994 38997 39014 39037 39053 39057 39072 39074 39097 39102 39124 39190 39196 39203 39206 39217 39224 39233 39235 39237 39242 39244 39263 39278 39287 39293 39294 39314 39315 39324 39337 39340 39342 39344 39351 39354 39355 39358 39379 39396 39397 39431 39432 39438 39444 39446 39456 39464 39470 39477 39487 39488 39496 39503 39525 39528 39534 39535 39540 39544 39548 39564 39568 39570 39594 39595 39613 39615 39623 39644 39690 39694 39700 39708 39709 39713 39717 39736 39777 39791 39798 39800 39805 39815 39819 39855 39868 39870 39905 39931 39938 39956 39957 39958 39978 39988 40000 40004 40013 40028 40075 40081 40093 40111 40128 40130 40133 40135 40136 40149 40156 40157 40165 40177 40194 40211 40233 40248 40251 40260 40261 40266 40288 40294 40307 40308 40338 40343 40366 40372 40393 40403 40405 40410 40411 40415 40424 40429 40435 40443 40452 40459 40463 40469 40470 40476 40495 40497 40500 40507 40511 40513 40514 40527 40528 40537 40539 40582 40591 40611 40616 40647 40652 40655 40656 40666 40668 40675 40696 40699 40702 40729 40731 40755 40758 40763 40773 40793 40795 40814 40816 40824 40833 40881 40895 40900 40907 40908 40910 40913 40918 40923 40933 40934 40948 40962 40963 40970 40991 41016 41025 41038 41063 41082 41090 41116 41128 41133 41140 41145 41149 41151 41152 41159 41165 41168 41192 41204 41208 41212 41231 41233 41250 41251 41259 41262 41300 41318 41324 41344 41349 41357 41360 41380 41382 41388 41390 41407 41433 41457 41480 41488 41490 41496 41497 41502 41515 41529 41535 41542 41557 41564 41567 41570 41571 41587 41588 41589 41616 41623 41628 41634 41644 41652 41653 41682 41707 41732 41750 41788 41794 41848 41862 41895 41955 41960 41985 42010 42028 42031 42037 42047 42048 42052 42056 42078 42099 42107 42127 42172 42175 42179 42207 42228 42232 42242 42245 42268 42300 42304 42318 42335 42338 42348 42355 42358 42374 42387 42389 42405 42414 42439 42440 42442 42447 42452 42481 42494 42501 42523 42531 42537 42538 42539 42560 42573 42580 42581 42614 42624 42630 42636 42650 42665 42667 42671 42677 42685 42694 42714 42719 42724 42747 42754 42766 42767 42777 42779 42790 42795 42796 42829 42832 42838 42859 42861 42875 42882 42894 42911 42919 42921 42922 42926 42942 42943 42949 42950 42962 42963 42965 42966 42970 42974 42979 42984 42996 43010 43024 43036 43067 43076 43079 43092 43095 43098 43101 43105 43112 43113 43139 43173 43174 43186 43190 43191 43216 43221 43229 43233 43261 43265 43269 43296 43301 43312 43323 43347 43351 43374 43377 43379 43392 43397 43401 43409 43414 43415 43425 43426 43432 43441 43445 43448 43458 43471 43478 43479 43505 43525 43530 43541 43582 43591 43625 43628 43635 43639 43649 43668 43670 43689 43694 43726 43739 43747 43748 43751 43755 43776 43789 43791 43805 43807 43809 43814 43843 43852 43855 43862 43874 43911 43919 43920 43929 43939 43960 43963 43966 43975 43985 43986 43987 44017 44060 44075 44076 44082 44088 44099 44100 44115 44117 44122 44129 44133 44147 44150 44156 44178 44182 44190 44191 44207 44211 44221 44232 44244 44247 44249 44257 44265 44284 44292 44293 44300 44306 44311 44335 44362 44368 44397 44401 44403 44411 44418 44422 44429 44430 44452 44474 44489 44515 44526 44548 44558 44560 44565 44566 44591 44601 44613 44623 44635 44650 44655 44663 44666 44671 44687 44693 44710 44714 44732 44734 44745 44752 44755 44756 44764 44790 44800 44806 44810 44824 44855 44879 44932 44935 44937 44938 44945 44965 44978 44993 45002 45005 45006 45007 45020 45051 45056 45062 45072 45083 45090 45093 45108 45126 45159 45161 45163 45181 45206 45216 45218 45232 45254 45261 45275 45277 45280 45290 45296 45299 45323 45328 45329 45331 45335 45368 45381 45396 45402 45435 45440 45446 45453 45470 45487 45489 45491 45492 45510 45511 45524 45529 45530 45538 45547 45553 45560 45574 45584 45607 45615 45625 45627 45633 45647 45655 45659 45661 45670 45673 45680 45689 45696 45753 45760 45763 45771 45799 45803 45811 45814 45831 45835 45839 45849 45850 45852 45855 45884 45887 45889 45891 45902 45903 45904 45905 45915 45927 45931 45970 45984 45995 45997 45998 46007 46014 46022 46049 46057 46066 46067 46070 46072 46095 46097 46102 46111 46114 46127 46137 46139 46140 46146 46148 46170 46173 46178 46193 46194 46199 46204 46206 46212 46213 46220 46237 46242 46263 46264 46293 46296 46312 46344 46357 46371 46374 46383 46389 46390 46397 46400 46401 46402 46411 46418 46421 46431 46436 46447 46460 46471 46472 46475 46480 46482 46484 46492 46493 46505 46517 46527 46531 46544 46558 46560 46568 46573 46582 46584 46594 46633 46635 46639 46656 46659 46666 46674 46679 46682 46684 46692 46693 46696 46708 46714 46732 46744 46750 46758 46798 46810 46811 46850 46881 46919 46926 46930 46938 46985 46990 47012 47019 47020 47027 47046 47050 47053 47065 47081 47094 47099 47111 47121 47122 47130 47138 47152 47163 47172 47195 47198 47220 47223 47225 47241 47244 47245 47259 47268 47280 47292 47298 47304 47312 47329 47335 47360 47365 47370 47371 47385 47392 47404 47429 47437 47445 47446 47468 47498 47514 47529 47530 47561 47562 47565 47569 47576 47578 47583 47594 47607 47632 47633 47643 47651 47656 47658 47678 47694 47710 47719 47724 47753 47764 47776 47778 47800 47808 47811 47815 47839 47855 47862 47871 47896 47904 47905 47911 47913 47923 47924 47932 47933 47934 47945 47971 47985 47995 48004 48018 48026 48043 48047 48069 48074 48077 48079 48087 48105 48114 48126 48127 48141 48151 48160 48173 48177 48206 48210 48212 48217 48236 48255 48285 48307 48357 48367 48369 48374 48378 48389 48400 48417 48420 48429 48441 48449 48450 48452 48460 48463 48475 48490 48495 48496 48498 48501 48503 48507 48520 48523 48526 48563 48574 48576 48579 48582 48584 48609 48618 48620 48626 48636 48643 48649 48650 48660 48666 48678 48684 48694 48701 48703 48741 48765 48766 48770 48775 48780 48784 48787 48790 48802 48815 48820 48833 48845 48849 48853 48858 48871 48872 48879 48881 48896 48972 48978 48983 48984 48992 48997 49010 49017 49049 49055 49059 49066 49087 49102 49114 49117 49120 49121 49122 49145 49147 49154 49165 49168 49176 49202 49203 49204 49214 49220 49236 49239 49254 49257 49267 49277 49284 49285 49300 49316 49330 49342 49356 49380 49397 49404 49412 49415 49419 49443 49444 49447 49453 49459 49476 49479 49485 49488 49504 49513 49526 49531 49532 49572 49573 49575 49581 49594 49595 49609 49654 49675 49680 49694 49696 49699 49701 49706 49711 49713 49760 49782 49789 49791 49795 49798 49808 49818 49821 49837 49839 49853 49879 49883 49884 49885 49899 49905 49923 49932 49934 49940 49958 49961 49966 49970 50054 50060 50061 50097 50104 50105 50110 50111 50119 50134 50140 50152 50158 50162 50171 50178 50185 50191 50232 50234 50249 50259 50264 50280 50281 50288 50293 50297 50302 50347 50362 50365 50370 50414 50470 50481 50487 50491 50495 50502 50510 50511 50526 50543 50560 50561 50567 50571 50573 50585 50596 50600 50607 50612 50618 50630 50637 50652 50655 50661 50689 50692 50700 50702 50708 50722 50737 50745 50751 50763 50768 50787 50812 50820 50829 50841 50844 50856 50865 50870 50880 50886 50892 50893 50896 50905 50926 50933 50934 50950 50951 50959 50969 50975 50977 50986 50990 51006 51008 51013 51027 51031 51035 51049 51054 51096 51100 51102 51110 51113 51123 51124 51130 51131 51134 51138 51140 51147 51149 51150 51157 51160 51163 51172 51176 51179 51187 51196 51202 51210 51222 51223 51233 51240 51247 51250 51254 51291 51297 51298 51300 51308 51309 51312 51317 51318 51319 51346 51359 51364 51369 51385 51389 51391 51404 51406 51407 51431 51432 51440 51450 51461 51468 51471 51483 51487 51489 51500 51511 51522 51554 51571 51579 51581 51592 51603 51609 51610 51619 51623 51627 51637 51644 51655 51661 51673 51675 51679 51683 51695 51697 51719 51722 51729 51731 51739 51745 51751 51764 51768 51772 51779 51789 51808 51817 51830 51837 51844 51846 51855 51859 51866 51887 51904 51915 51922 51926 51944 51969 51973 51976 51992 51998 52007 52033 52080 52091 52095 52102 52103 52105 52111 52133 52145 52161 52166 52169 52182 52207 52216 52246 52256 52258 52263 52264 52272 52273 52281 52303 52326 52329 52349 52350 52367 52370 52375 52383 52387 52390 52404 52440 52445 52489 52491 52492 52496 52500 52502 52513 52527 52536 52540 52543 52551 52561 52571 52590 52596 52607 52616 52625 52628 52635 52637 52644 52661 52675 52677 52679 52685 52720 52743 52745 52751 52758 52761 52773 52795 52821 52843 52852 52872 52873 52879 52905 52913 52914 52922 52923 52929 52940 52945 52951 52952 52957 52966 52985 53003 53020 53026 53051 53056 53065 53076 53089 53094 53127 53129 53132 53135 53158 53184 53211 53212 53226 53230 53233 53242 53243 53267 53278 53285 53293 53304 53310 53313 53323 53335 53344 53345 53366 53375 53388 53393 53397 53407 53410 53417 53420 53459 53463 53472 53481 53483 53498 53514 53522 53524 53525 53546 53555 53576 53581 53593 53597 53601 53609 53613 53615 53623 53625 53648 53671 53672 53691 53703 53706 53713 53728 53755 53765 53800 53803 53807 53811 53819 53831 53855 53861 53863 53870 53886 53890 53911 53925 53932 53960 53966 53968 53969 53993 53996 54013 54021 54038 54043 54053 54069 54080 54085 54120 54122 54129 54133 54135 54143 54144 54153 54164 54168 54181 54188 54192 54199 54213 54224 54229 54232 54249 54251 54263 54264 54269 54272 54288 54301 54305 54306 54311 54318 54340 54355 54357 54365 54376 54379 54381 54384 54393 54416 54463 54485 54493 54499 54510 54523 54531 54534 54567 54570 54571 54572 54573 54575 54580 54590 54599 54604 54639 54672 54688 54700 54716 54723 54738 54753 54764 54767 54768 54775 54784 54802 54809 54816 54819 54825 54835 54847 54858 54868 54873 54884 54888 54898 54933 54934 54947 54949 54960 54970 54984 54994 55023 55028 55029 55034 55038 55044 55046 55062 55078 55098 55113 55125 55143 55145 55152 55166 55213 55222 55224 55232 55234 55260 55261 55266 55270 55272 55279 55302 55307 55329 55330 55347 55372 55373 55389 55392 55408 55410 55416 55419 55433 55437 55441 55446 55462 55500 55501 55512 55530 55567 55570 55575 55577 55579 55595 55596 55604 55616 55624 55634 55639 55652 55667 55671 55684 55690 55701 55705 55710 55717 55719 55760 55762 55763 55775 55801 55823 55825 55836 55867 55871 55884 55905 55910 55959 55960 55963 55987 55989 55997 56001 56004 56019 56029 56034 56049 56056 56058 56062 56124 56165 56187 56190 56192 56208 56219 56230 56235 56240 56267 56272 56284 56286 56301 56305 56333 56343 56344 56345 56346 56347 56370 56373 56379 56381 56384 56385 56388 56398 56400 56403 56405 56410 56416 56419 56430 56474 56475 56489 56491 56503 56508 56510 56515 56521 56537 56539 56544 56548 56573 56574 56588 56596 56597 56607 56620 56628 56633 56649 56661 56672 56689 56715 56716 56729 56735 56745 56749 56761 56768 56774 56793 56809 56844 56858 56870 56898 56906 56927 56930 56943 56950 56963 56990 57012 57025 57027 57028 57044 57050 57052 57056 57084 57089 57101 57102 57113 57116 57129 57137 57157 57158 57173 57203 57205 57207 57225 57243 57246 57254 57258 57262 57275 57297 57301 57307 57326 57329 57333 57343 57388 57391 57422 57424 57431 57442 57448 57465 57468 57471 57478 57506 57513 57544 57563 57569 57574 57580 57610 57624 57630 57632 57638 57664 57669 57673 57674 57682 57696 57698 57703 57708 57725 57733 57771 57778 57797 57805 57812 57823 57824 57829 57846 57852 57866 57877 57896 57897 57900 57908 57909 57937 57961 57982 57999 58000 58001 58004 58015 58020 58028 58031 58048 58065 58072 58073 58080 58085 58091 58100 58110 58112 58133 58143 58145 58146 58156 58164 58166 58180 58188 58191 58194 58198 58199 58200 58238 58251 58254 58261 58278 58340 58341 58369 58376 58377 58379 58383 58396 58397 58404 58407 58409 58425 58431 58436 58450 58468 58488 58491 58500 58501 58504 58508 58509 58510 58522 58523 58526 58527 58534 58536 58541 58549 58558 58588 58598 58599 58603 58615 58624 58647 58678 58690 58694 58700 58701 58702 58710 58731 58759 58766 58790 58793 58802 58807 58814 58822 58825 58837 58838 58840 58846 58852 58854 58856 58869 58872 58886 58889 58907 58911 58924 58937 58939 58940 58951 58955 58964 58968 58974 58984 58997 58998 59000 59014 59016 59017 59021 59024 59032 59062 59077 59082 59087 59088 59109 59114 59116 59117 59146 59156 59161 59170 59176 59188 59190 59198 59207 59211 59215 59217 59223 59228 59244 59260 59266 59286 59287 59293 59295 59303 59307 59316 59317 59318 59324 59325 59339 59342 59350 59365 59368 59386 59389 59397 59399 59400 59406 59413 59414 59416 59424 59425 59431 59459 59471 59477 59495 59498 59508 59510 59522 59539 59568 59591 59599 59610 59613 59618 59653 59654 59656 59673 59678 59679 59690 59731 59739 59759 59767 59769 59779 59790 59805 59817 59822 59829 59833 59835 59844 59874 59886 59899 59906 59916 59917 59920 59924 59925 59942 59954 59968 59971 60009 60031 60033 60036 60044 60052 60056 60058 60061 60067
9000
65000
```

output

```
True
```

Private Test case 5

input

```
25 27 75 77 157 210 221 234 240 241 316 396 435 467 557 643 643 716 720 798 824 893 929 979 1054 1067 1260 1322 1391 1393 1540 1580 1770 1794 1824 1835 1915 1983 1996 2034 2036 2086 2184 2311 2327 2354 2367 2372 2390 2401 2489 2490 2500 2523 2528 2582 2697 2732 2763 2779 2838 2976 2985 2990 3007 3098 3115 3163 3191 3269 3306 3439 3502 3505 3536 3573 3590 3645 3702 3705 3861 3913 4144 4238 4244 4363 4407 4667 4689 4767 4854 4894 4985 5000 5001 5068 5119 5309 5339 5398 5496 5533 5562 5575 5589 5661 5704 5786 5829 5924 5957 6015 6103 6133 6214 6226 6244 6258 6276 6306 6318 6327 6363 6388 6406 6436 6454 6572 6735 6881 6952 6966 6971 7041 7070 7093 7248 7275 7283 7307 7308 7330 7352 7365 7390 7436 7516 7531 7585 7600 7626 7660 7731 7742 7758 7791 7848 7871 7872 7898 7932 7972 8017 8033 8089 8127 8280 8324 8390 8425 8471 8485 8553 8636 8713 8781 8931 8990 8999 9024 9087 9107 9107 9188 9213 9218 9228 9285 9372 9456 9556 9568 9574 9574 9678 9816 9848 9954 9968 9985 45 53 71 79 98 112 119 131 164 173 184 210 245 286 359 391 398 418 446 449 460 462 467 472 499 509 511 525 527 547 570 575 579 634 641 655 661 679 692 715 719 757 771 806 808 854 873 919 922 961 1051 1052 1060 1121 1165 1187 1221 1245 1251 1252 1287 1343 1362 1363 1384 1460 1471 1533 1555 1587 1594 1608 1612 1638 1730 1730 1766 1766 1767 1774 1843 1865 1872 1874 1925 1982 2010 2113 2114 2121 2132 2148 2175 2191 2228 2274 2279 2280 2326 2337 2443 2455 2464 2467 2494 2502 2567 2570 2584 2586 2587 2595 2597 2617 2659 2689 2712 2722 2724 2725 2733 2751 2752 2757 2813 2878 2914 2940 2943 2946 2955 2965 2987 2998 3050 3068 3108 3129 3154 3171 3225 3274 3281 3298 3309 3318 3329 3335 3351 3354 3357 3405 3413 3440 3486 3490 3495 3505 3518 3521 3541 3543 3548 3560 3588 3605 3612 3614 3655 3683 3689 3709 3734 3745 3770 3778 3783 3787 3788 3811 3825 3828 3838 3850 3858 3863 3871 3884 3887 3896 3907 3920 3969 3995 4030 4037 4077 4089 4115 4139 4159 4169 4178 4204 4220 4253 4258 4267 4279 4280 4290 4343 4373 4385 4403 4414 4433 4446 4466 4480 4493 4521 4533 4538 4563 4588 4622 4641 4645 4656 4715 4726 4749 4783 4797 4799 4866 4877 4881 4911 4916 4961 4970 4990 5005 5019 5030 5039 5045 5054 5076 5077 5093 5102 5119 5131 5135 5154 5166 5178 5179 5204 5252 5259 5276 5316 5326 5345 5374 5375 5397 5406 5410 5416 5418 5433 5442 5492 5500 5531 5565 5568 5571 5714 5719 5757 5768 5813 5822 5832 5839 5865 5877 5905 5925 5926 5958 5967 5968 5988 6008 6027 6029 6033 6055 6077 6086 6087 6088 6111 6128 6133 6146 6159 6199 6218 6237 6260 6316 6354 6377 6420 6420 6457 6468 6473 6550 6553 6564 6570 6573 6590 6645 6645 6682 6694 6697 6698 6728 6747 6751 6760 6762 6765 6778 6779 6788 6794 6818 6821 6824 6844 6844 6892 6906 6938 6968 7029 7040 7041 7063 7095 7101 7102 7119 7142 7200 7222 7225 7231 7233 7243 7253 7254 7297 7303 7315 7317 7328 7365 7372 7389 7391 7410 7439 7445 7458 7464 7476 7540 7571 7585 7628 7628 7631 7642 7643 7711 7728 7734 7749 7750 7755 7759 7768 7826 7836 7881 7902 7950 7997 8000 8004 8026 8037 8042 8077 8096 8116 8127 8135 8148 8170 8189 8191 8223 8223 8236 8258 8278 8285 8296 8315 8319 8322 8331 8358 8373 8404 8423 8435 8466 8489 8490 8494 8531 8543 8548 8585 8613 8639 8698 8703 8706 8708 8735 8740 8743 8754 8756
200
2002
```

output

```
True
```

<div style="page-break-after: always;"></div>

## Week-6 Problem 2 ##

Function **heapSort(arr)** sorts the array `arr` using max heap. Complete the function **heapify(arr, n, i)**, that takes three arguments, `arr` is the max heap array, `n` is the number of elements in heap `arr` and `i` is the index of element that needs to be heapified and heapifies the array from index `0` to `n-1` with respect to element at index`i`.

```python
def heapify(arr, n, i):
  #Complete this function

def heapSort(arr):
    n = len(arr)
    for i in range(n//2, -1, -1):
        heapify(arr, n, i)
    for i in range(n-1, 0, -1):
        arr[i], arr[0] = arr[0], arr[i]
        heapify(arr, i, 0)
```

sample input

 ```
45 23 6 12 9 1 22 58
 ```

sample output

```
1 6 9 12 22 23 45 58 
```

### Solution

```python
def heapify(arr, n, i):
    largest = i
    l = 2 * i + 1
    r = 2 * i + 2
    if l < n and arr[i] < arr[l]:
        largest = l
    if r < n and arr[largest] < arr[r]:
        largest = r
    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]
        heapify(arr, n, largest)
```

**Suffix**

```python
def heapSort(arr):
    n = len(arr)
    for i in range(n//2, -1, -1):
        heapify(arr, n, i)
    for i in range(n-1, 0, -1):
        arr[i], arr[0] = arr[0], arr[i]
        heapify(arr, i, 0)

L = [int(item) for item in input().split(" ")]
heapSort(L)
print(*L)
```

**Public test case 2**

**Input**

```
45 23 6 12 9 1 22 58
```

**Output**

```
1 6 9 12 22 23 45 58 
```

**Public test case 2**

**Input**

```
45 22 57 12 33 73 55 2 80 62
```

**Output**

```
2 12 22 33 45 55 57 62 73 80
```

**Private test case 1**

**Input**

```
45 23 6 12 9 1 22 58
```

**Output**

```
1 6 9 12 22 23 45 58 
```

**Private test case 2**

**Input**

```
45 22 57 12 33 73 55 2 80 62
```

**Output**

```
2 12 22 33 45 55 57 62 73 80
```

**Private test case 3**

**Input**

```
284 276 201 126 463 475 234 319 343 356 441 68 335 101 268 71 202 363 82 90 380 349 198 31 479 471 28 95 224 400 259 39 66 254 70 80 183 107 343 15 434 204 387 370 280
```

**Output**

```
15 28 31 39 66 68 70 71 80 82 90 95 101 107 126 183 198 201 202 204 224 234 254 259 268 276 280 284 319 335 343 343 349 356 363 370 380 387 400 434 441 463 471 475 479
```

**Private test case 4**

**Input**

```
1246 1423 646 36 1179 1022 159 76 1026 847 1214 830 1018 846 737 1164 423 1421 369 360 1208 711 618 760 664 685 1250 623 127 139 30 948 966 660 1262 1137 1176 959 942 1494 1213 1057 1240 136 954 293 251 692 5 254 563 162 181 1103 898 153 1286 1148 628 8 214 460 40 152 487 627 1164 1098 609 643 1265 1454 907 1194 1127 593 1155 1120 160 202 39 252 762 1005 1338 858 435 1252 803 872 1068 86 1098 275 836
```

**Output**

```
5 8 30 36 39 40 76 86 127 136 139 152 153 159 160 162 181 202 214 251 252 254 275 293 360 369 423 435 460 487 563 593 609 618 623 627 628 643 646 660 664 685 692 711 737 760 762 803 830 836 846 847 858 872 898 907 942 948 954 959 966 1005 1018 1022 1026 1057 1068 1098 1098 1103 1120 1127 1137 1148 1155 1164 1164 1176 1179 1194 1208 1213 1214 1240 1246 1250 1252 1262 1265 1286 1338 1421 1423 1454 1494
```

**Private test case 5**

**Input**

```
13119 3066 7450 8605 3059 7586 14747 4488 14219 12774 9409 2658 14338 12196 13622 11005 10273 10069 14784 14275 11451 10232 2325 11572 4172 4830 12502 2961 6152 14738 7465 2131 10200 4896 10890 10525 161 5356 5792 10723 1563 6397 6588 10048 2744 548 14995 3136 14092 169 4338 6999 14244 11584 4838 9937 12128 14681 5201 1609 9085 2001 7509 6875 3010 5456 12611 11972 4096 12131 8424 7680 2060 2804 10561 11208 7376 11734 4514 13269 7038 10287 7884 14685 13821 6251 189 13507 13660 6941 7497 13957 974 14458 5153 5650 11644 6809 12643 7517 6979 14501 5413 1335 8476 7586 2048 10099 1693 14227 7977 8643 12315 13632 3357 9132 10987 10042 1143 13162 14497 11321 14058 8660 7817 104 8859 2909 869 14164 9418 10292 5500 1994 11055 13146 5434 1520 950 10091 14592 7637 12639 1802 2677 2265 11810 14782 1725 8482 12738 4977 10559 7869 7445 7767 12552 10172 4521 1958 4536 1299 8008 13855 7372 187 3390 7349 13123 4241 14125 7384 175 14390 14383 8940 7302 10516 4589 7054 11492 2261 6681 6633 12187 5611 2235 3255 6865 1787 13112 11703 1810 8649 13705 10045 14750 6975 12006 11945 14680 585 6935 7023 3638 773 12605 9139 12868 10719 3503 5645 7429 14093 4847 7462 7695 5674 14588 4977 7200 585 4884 4663 7095 13041 334 4705 7063 3787 12631 14273 11698 8831 12214 1470 13827 8217 9588 14463 12238 12398 9334 2461 8001 7345 11112 6017 7961 3701 9354 7072 3353 4046 670 9725 4668 10399 7718 14196 4568 7734 7538 11506 2610 6076 1522 6842 13882 10333 8071 12185 3718 11455 909 14908 14638
```

**Output**

```
104 161 169 175 187 189 334 548 585 585 670 773 869 909 950 974 1143 1299 1335 1470 1520 1522 1563 1609 1693 1725 1787 1802 1810 1958 1994 2001 2048 2060 2131 2235 2261 2265 2325 2461 2610 2658 2677 2744 2804 2909 2961 3010 3059 3066 3136 3255 3353 3357 3390 3503 3638 3701 3718 3787 4046 4096 4172 4241 4338 4488 4514 4521 4536 4568 4589 4663 4668 4705 4830 4838 4847 4884 4896 4977 4977 5153 5201 5356 5413 5434 5456 5500 5611 5645 5650 5674 5792 6017 6076 6152 6251 6397 6588 6633 6681 6809 6842 6865 6875 6935 6941 6975 6979 6999 7023 7038 7054 7063 7072 7095 7200 7302 7345 7349 7372 7376 7384 7429 7445 7450 7462 7465 7497 7509 7517 7538 7586 7586 7637 7680 7695 7718 7734 7767 7817 7869 7884 7961 7977 8001 8008 8071 8217 8424 8476 8482 8605 8643 8649 8660 8831 8859 8940 9085 9132 9139 9334 9354 9409 9418 9588 9725 9937 10042 10045 10048 10069 10091 10099 10172 10200 10232 10273 10287 10292 10333 10399 10516 10525 10559 10561 10719 10723 10890 10987 11005 11055 11112 11208 11321 11451 11455 11492 11506 11572 11584 11644 11698 11703 11734 11810 11945 11972 12006 12128 12131 12185 12187 12196 12214 12238 12315 12398 12502 12552 12605 12611 12631 12639 12643 12738 12774 12868 13041 13112 13119 13123 13146 13162 13269 13507 13622 13632 13660 13705 13821 13827 13855 13882 13957 14058 14092 14093 14125 14164 14196 14219 14227 14244 14273 14275 14338 14383 14390 14458 14463 14497 14501 14588 14592 14638 14680 14681 14685 14738 14747 14750 14782 14784 14908 14995
```



