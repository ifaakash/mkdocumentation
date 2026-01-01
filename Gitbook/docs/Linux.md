## Command short for writing multiple Linux commands
| Symbol | Meaning                                   |
| ------ | ----------------------------------------- |
| `;`    | Run next command no matter what           |
| `&&`   | Run next only if previous succeeded       |
| `&`    | Run command in background                 |
| `\`    | Line continuation (don’t end command yet) |

```
# Runs the second command regardless of whether the first one succeeds.
cd docs; ls

# Runs the second command only if the first one succeeds.
cd docs && ls
```
