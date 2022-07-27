Based on https://github.com/tedeschi/MajoranaSW4.2/blob/master/mjsw/Dockerfile.mjsw

Updated MGDO needs new ROOT_INCLUDE_PATH entry

Still have an unknown error - typically needs expanded ROOT_INCLUDE_PATH, but these are ROOT/CLHEP headers...
```
Error in cling::AutoLoadingVisitor::InsertIntoAutoLoadingState:
   Missing FileEntry for ThreeVector.h
   requested to autoload type CLHEP::Hep3Vector
Error in cling::AutoLoadingVisitor::InsertIntoAutoLoadingState:
   Missing FileEntry for IFunctionfwd.h
   requested to autoload type ROOT::Math::IGradientFunctionMultiDim
Error in cling::AutoLoadingVisitor::InsertIntoAutoLoadingState:
   Missing FileEntry for IFunctionfwd.h
   requested to autoload type ROOT::Math::IMultiGradFunction
```
