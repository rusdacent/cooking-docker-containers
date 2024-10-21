# Semgrep demo

## Install Semgrep

https://github.com/semgrep/semgrep#option-2-getting-started-from-the-cli

## Example

> [!NOTE]
> Dockerfile примера взят в виде последнего релиза для Semgrep https://raw.githubusercontent.com/semgrep/semgrep/refs/tags/v1.92.0/Dockerfile

Запустим проверку

```
$ semgrep --config "p/dockerfile" semgrep/Dockerfile
```

или

```
$ docker run --rm -v "${PWD}/semgrep:/src" semgrep/semgrep semgrep --config "p/dockerfile" Dockerfile
```

Результаты
```
┌─────────────┐
│ Scan Status │
└─────────────┘
  Scanning 1 file tracked by git with 5 Code rules:
  Scanning 1 file with 5 dockerfile rules.
                  
                  
┌────────────────┐
│ 1 Code Finding │
└────────────────┘
              
    Dockerfile
   ❯❯❱ dockerfile.security.missing-user.missing-user
    By not specifying a USER, a program in the container may run as 'root'. This is a security hazard.
    If an attacker can control a process running as root, they may have control over the container.   
    Ensure that the last USER in a Dockerfile is a USER other than 'root'.                            
    Details: https://sg.run/Gbvn                                                                          
           ▶▶┆ Autofix ▶ USER non-root CMD ["semgrep", "--help"]
          285┆ CMD ["semgrep", "--help"]

┌──────────────┐
│ Scan Summary │
└──────────────┘

Ran 5 rules on 1 file: 1 finding.
```

Перепроверяем

```
$ docker run --rm semgrep/semgrep id
uid=0(root) gid=0(root) groups=0(root),1(bin),2(daemon),3(sys),4(adm),6(disk),10(wheel),11(floppy),20(dialout),26(tape),27(video)
```