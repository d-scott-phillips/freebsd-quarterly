## sysctlinfo ##

Contact: Alfonso Sabato Siciliano <alfonso.siciliano@email.com>  

Link:	 [gitlab.com/alfix/sysctlinfo](https://gitlab.com/alfix/sysctlinfo)    

The FreeBSD kernel maintains a Management Information Base (MIB) where a 
component (object) represents a parameter of the system. The _sysctl_ system 
call explores the MIB to find an object by its Object Identifier (OID) and 
calls its handler to get or set the value of the parameter. It is often 
necessary to find an object not to call its handler but to get its info 
(description, type, name, next object, etc.), so the kernel provides an 
undocumented interface implemented in kern\_sysctl.c.

sysctlinfo is a new interface to explore the sysctl MIB and to pass the info
of an object to the userland. The project provides: a README, a manual, helper 
macros, examples, tests and converted tools.

Primarily sysctlinfo provides a new set of sysctl nodes (corresponding to the 
kernel interface) to handle an object up to CTL\_MAXNAME levels:
sysctl.entryfakename, sysctl.entrydesc, sysctl.entrylabel, sysctl.entrykind, 
sysctl.entryfmt and sysctl.entrynextleaf. Moreover new features have been 
implemented: the support for the capability mode, sysctl.entryname, 
sysctl.entryidbyname and sysctl.entrynextnode. To get all the info about an 
object the kernel needs to find it many times, then the new 
sysctl.entryallinfo\* nodes were written, they are 30% more efficient. Finally, 
\*byname nodes were added: they search the object by its name avoiding to call 
sysctl.name2oid (or similar) to explore the MIB just to find the corresponding 
OID.

sysctlinfo can be installed via _sysutils/sysctlinfo-kmod_ or by applying the 
_sysctlinfo.diff_ patch (more efficient than the module because uses a shared 
lock). The interface is used by _deskutils/sysctlview 1.5_, 
_sysutils/nsysctl 1.2_ and the converted version of sysctl(8), sysctlbyname(3), 
sysctlnametomib(3), they should be used to get the value of an object with 23/24 
levels or if some level-name has only the '\0' character. In the future a new 
_byname_ node will be added to allow sysctlbyname() to manage a CTLTYPE_NODE with
a no-NULL handler, example sysctlbyname("kern.proc.pid.\<pid\>").
