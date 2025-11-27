# Dotnet 8 unter Linux 

  * Dotnet 8 kann am besten über den Paket-Manager installiert werden

```
sudo apt search dotnet8
sudo apt search dotnet-runtime-8.0
```

```
sudo apt install dotnet-runtime-8.0 -y
# zum Kompilieren brauche ich noch die sdk 
sudo apt install dotnet-sdk-8.0 -y
```

 * Es werden noch ein Reihen von Abhängigkeiten installiert

## Now dowload an sample and compile it (run it)

  * In addition it will also create an executable 

```
# This takes a while 
cd
git clone https://github.com/dotnet/samples.git
cd ~/samples/csharp/getting-started/console-webapiclient
dotnet run 
```
