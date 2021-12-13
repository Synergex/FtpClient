
SUBMISSION AUTHOR:      Tod Phillips
                        mail to: tod.phillips@synergex.com
                        Synergex
                        2330 Gold Meadow Way
                        Gold River, CA 95670
                        Phone: 916-635-7300
                        http://www.synergex.com

SUBMISSION NAME:        FtpClient

PLATFORM:               Windows, VMS, Unix

SYNERGY VERSION:        Synergy v9.1.5 or higher

MODIFICATION HISTORY:   May 19, 2011

DESCRIPTION:            FtpClient is a class that allows basic FTP operations with a remote FTP server.
                        The class supports binary and ascii transfer modes, and both Active or Passive
                        server connections.

ADDITIONAL NOTES:       In order to use this class, both it and its dependency classes must first be
                        prototyped with the DBLPROTO utility.  Ensure that your SYNIMPDIR and SYNEXPDIR
                        environment variables have been set, then run the protyper by typing:

                                dblproto <filename>.dbc

                        from a command prompt in the directory where the source files have
                        been saved. (Alternately, import the files into a Workbench project, right-
                        click the project in the Projects tab and select "Generate Synergy
                        Protypes..."). The included files can then be compiled and added to any
                        library or ELB.  To use the provided class methods, simply type

                                import SynPSG.System.Net

                        at the top of your source code. (See FTP_Main_Program, included in download).
                        Depending on the version of Synergy used for the compilation, it may also be
                        necessary to import the class's dependency namespaces:

                                import SynPSG.System
                                import SynPSG.System.IO
                                import SynPSG.System.Net.Sockets
                                import SynPSG.System.Net.Mime
                                import System.collections


CLASS:                  FtpClient      (Public)

ENUMERATION(S):
        Public Enumeration FtpConnectMode
                Active
                Passive
        Public Enumeration FtpTransferType
                Ascii
                Binary

CONSTRUCTOR:
        FtpClient (Public)
                Overloaded. Initializes a new instance of the FtpClient class.

PUBLIC FIELDS:
        ConnectMode
        Host
        LogFile
        Port
        Pwd
        User
        TransferType
        RemoteDirectory
        RemoteDirListing

PUBLIC PROPERTIES:
        IsConnected
        LoggingEnabled
        LocalDirectory
        LastServerResponse

PUBLIC METHOD(S):
        Connect (Overloaded)
        Disconnect
        ChangeDirectory
        DeleteFile
        GetDirectoryListing (Overloaded)
        GetFile (Overloaded)
        PutFile (Overloaded)
        RenameFile (Overloaded)

EXAMPLE(S):

        The following program demonstrates the use of the SynDateTime class; see the
        included Workbench Project and source code files for FTP_Main_Program as 
        another example. This project can be built and executed as it is.

;; Program to demonstrate the FtpClient class.

import SynPSG.System
import SynPSG.System.IO
import SynPSG.System.Net
import SynPSG.System.Net.Ftp
import SynPSG.System.Net.Sockets
import SynPSG.System.Net.Mime

main
record
    myFTP   ,@synpsg.system.net.ftp.FtpClient
endrecord

proc
        open(1,i,"TT:")
        myFtp = new ftpclient( "ftp.someserver.com", 21, FtpConnectMode.Passive)
        myFtp.LoggingEnabled = true
	myFtp.LogFile = "C:\temp\ftp.log"

	writes(1,'...Attempting to connect...')

        ; Defaults to "anonymous" login when no username or password passed
	myFtp.Connect()

	if !(myFTP.IsConnected) then
	begin
		writes(1,'...Connection failed!...')
	end
	else
	begin
		writes(1,'...Connection successful...')
		myFtp.LocalDirectory = "C:\temp"
		myFtp.ChangeDirectory("/somedir/somesubdir/")
                ; Get a file. Defaults to ASCII transfer mode.
		myFtp.GetFile("somefile.txt")
		writes(1,'...' + myFtp.LastServerResponse + '...')
		myFtp.Disconnect()
	end

endmain

