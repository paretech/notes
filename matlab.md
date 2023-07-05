# MATLAB

## Idioms

### More readable function arguments

MATLAB typically does not allow you to use named arguments. There are some exceptions, like "options" struct when using argument validation but those have some serious limitations. One work around is to use a struct to define a sequence of arguments, convert to cell array, then expand in function call. This method has some serious drawbacks but it is still none the less useful in certain applications. Some drawbacks are you are now maintaining two copies of the same data, this can be problematic during interactive debug when you think you've updated a parameter but havent. There are undoubtedly other drawbacks and ways the cited onces can cause pain. It is up to you to decide if the enhancements to readability are indeed worth the costs. When using libraries like Psychtoolbox that uses long lists of arguments some containing `[]` to represent default values it can easily be justified as the likelyhood of mixing up one or more arguments is high.

```MATLAB
  s = struct( ...
      win=obj.windowPointer, ...
      leftOffset=offset, ...
      leftScale=[1, 1], ...
      rightOffset=offset + [screenWidth/2, 0], ...
      rightScale=[1, 1], ...
      shaders=[], ...
      offsetUnit='pixels' ...
  );
  
  sCell = struct2cell(s);
  
  SetStereoSideBySideParametersTemp(sCell{:});
```

## Third Party

- [Advanced Logger](https://www.mathworks.com/matlabcentral/fileexchange/87322-advanced-logger-for-matlab)
- [TxtMenu](https://www.mathworks.com/matlabcentral/fileexchange/28285-txtmenu-text-based-menu-for-the-command-window?s_tid=FX_rc1_behav)
- 
