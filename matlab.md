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

### Generic "init" script
Save the following to a script named `init.m` at the base of project code (i.e. same level as any namespace packages). Add call to `savepath()` at end of `init.m` to make path additions persistent. 

The method of adding all directories in folder is nice, but sometimes overreaching. For example, many third party modules have documents, examples and other things. Using the generic method will place all these on the global path which may have undesired side effects. For more control, simply add the top level of each third-party module.

```MATLAB
%% Get base directory
baseDirectory = fileparts(which(mfilename('fullpath')));

%% Temporarily add namespace package to MATLAB path
addpath(baseDirectory);

%% Temporarily add third-party libraries to MATLAB path
% This will add all files in "third-party" directory to path
addpath(genpath(fullfile(baseDirectory, "third-party")));

% Alternatively, add each individual directory.
% addpath(fullfile(baseDirectory, "third-party/advancedLogger"));
% addpath(fullfile(baseDirectory, "third-party/txtmenu"));

% Temporarily add test directory to MATLAB path
addpath(genpath(fullfile(baseDirectory, "tests")));

% Temporarily add examples to MATLAB path
addpath(genpath(fullfile(baseDirectory, "examples")));
```

## Third Party

- [Advanced Logger](https://www.mathworks.com/matlabcentral/fileexchange/87322-advanced-logger-for-matlab)
- [TxtMenu](https://www.mathworks.com/matlabcentral/fileexchange/28285-txtmenu-text-based-menu-for-the-command-window?s_tid=FX_rc1_behav)

# Copy Command Output to Clipboard
```
function clip(command)
    % Copy output of command to clipboard.
    commandOutput = evalc(command);

    commandAndOutput = strjoin({command, commandOutput}, '\n');
    clipboard('copy', commandAndOutput);

    disp(commandOutput);
end
```

