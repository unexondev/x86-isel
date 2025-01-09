## Translation of GEP (getelementptr) into x86 machine code

# Resultant MIR for a GEP:
```
%104:gr64 = MOV64ri @.base
ADD64ri32 %104:gr64(tied-def 0), offset, implicit-def $eflags
```

# Piece of code performs translation for GEP
```c++
/* llvm/lib/CodeGen/SelectionDAG/FastISel.cpp */

// If this is a constant subscript, handle it quickly.
if (const auto *CI = dyn_cast<ConstantInt>(Idx)) {
if (CI->isZero())
    continue;
// N = N + Offset
uint64_t IdxN = CI->getValue().sextOrTrunc(64).getSExtValue();
TotalOffs += GTI.getSequentialElementStride(DL) * IdxN;
if (TotalOffs >= MaxOffs) {
    N = fastEmit_ri_(VT, ISD::ADD, N, TotalOffs, VT);
    if (!N) // Unhandled operand. Halt "fast" selection and bail.
    return false;
    TotalOffs = 0;
}
continue;
}
```

# Beginning of the corresponding function what does the previous code belong to
```c++
/* llvm/lib/CodeGen/SelectionDAG/FastISel.cpp */

Register N = getRegForValue(I->getOperand(0));
```
_This call sets a visual register to store the base value if it's not set._