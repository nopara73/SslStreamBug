UPDATE: This is fixed in .NET Core 3.1: https://github.com/nopara73/SslStreamBugCore3

# SslStreamBug

## How to run
0. Download Tor and run it: torproject.org/download/download.html.en
1. Note: If you run the Tor browser and not directly the Tor process then your default socks port will be 9150, instead of 9050 and you need to change it in Program.cs
2. `dotnet restore` 
3. `dotnet build`
4. `dotnet run`

## Where is the bug present?

The bug is present on Ubuntu, but not on Windows 7. I suspect it is an **issue on both Linux and OSX, but not an issue on Windows**, because it doesn't use OpenSSL.  
The bug is present in **`dotnet --version` output:** 1.0.1  and **`dotnet --version` output:** 2.0.0-preview2-006497  

## The stacktrace
  
```
Connecting socket...
Handshaking Tor...
Connecting to destination...
Ssl authenticating as client...

Unhandled Exception: System.AggregateException: One or more errors occurred. (A call to SSPI failed, see inner exception.) ---> System.Security.Authentication.AuthenticationException: A call to SSPI failed, see inner exception. ---> Interop+OpenSsl+SslException: SSL Handshake failed with OpenSSL error - SSL_ERROR_SSL. ---> Interop+Crypto+OpenSslCryptographicException: error:14077438:SSL routines:SSL23_GET_SERVER_HELLO:tlsv1 alert internal error
   --- End of inner exception stack trace ---
   at Interop.OpenSsl.DoSslHandshake(SafeSslHandle context, Byte[] recvBuf, Int32 recvOffset, Int32 recvCount, Byte[]& sendBuf, Int32& sendCount)
   at System.Net.SslStreamPal.HandshakeInternal(SafeFreeCredentials credential, SafeDeleteContext& context, SecurityBuffer inputBuffer, SecurityBuffer outputBuffer, Boolean isServer, Boolean remoteCertRequired)
   --- End of inner exception stack trace ---
   at System.Runtime.ExceptionServices.ExceptionDispatchInfo.Throw()
   at System.Net.Security.SslState.StartSendAuthResetSignal(ProtocolToken message, AsyncProtocolRequest asyncRequest, ExceptionDispatchInfo exception)
   at System.Net.Security.SslState.CheckCompletionBeforeNextReceive(ProtocolToken message, AsyncProtocolRequest asyncRequest)
   at System.Net.Security.SslState.StartSendBlob(Byte[] incoming, Int32 count, AsyncProtocolRequest asyncRequest)
   at System.Net.Security.SslState.ProcessReceivedBlob(Byte[] buffer, Int32 count, AsyncProtocolRequest asyncRequest)
   at System.Net.Security.SslState.ReadFrameCallback(AsyncProtocolRequest asyncRequest)
--- End of stack trace from previous location where exception was thrown ---
   at System.Runtime.ExceptionServices.ExceptionDispatchInfo.Throw()
   at System.Net.Security.SslState.InternalEndProcessAuthentication(LazyAsyncResult lazyResult)
   at System.Net.Security.SslState.EndProcessAuthentication(IAsyncResult result)
   at System.Net.Security.SslStream.EndAuthenticateAsClient(IAsyncResult asyncResult)
   at System.Threading.Tasks.TaskFactory`1.FromAsyncCoreLogic(IAsyncResult iar, Func`2 endFunction, Action`1 endAction, Task`1 promise, Boolean requiresSynchronization)
   --- End of inner exception stack trace ---
   at System.Threading.Tasks.Task.ThrowIfExceptional(Boolean includeTaskCanceledExceptions)
   at System.Threading.Tasks.Task.Wait(Int32 millisecondsTimeout, CancellationToken cancellationToken)
   at System.Threading.Tasks.Task.Wait()
   at SslStreamBug.Program.Main(String[] args) in /home/user/SslStreamBug/SslStreamBug/Program.cs:line 56
```
