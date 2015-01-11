import os

env = Environment()

debug = ARGUMENTS.get("debug", 0)


if env["PLATFORM"] == "win32":
    apachedir = ARGUMENTS.get('apachedir', "C:/Program Files/Apache Software Foundation/Apache2.2")

    if int(debug):
        env.Append(CCFLAGS = ["/Zi", "/Od", "/MDd"],
                   LINKFLAGS = ["/DEBUG"])
    else:
        env.Append(CCFLAGS = ["/O2", "/MD"])
    env.Append(CCFLAGS = ["/EHsc", "/W3"],
               CPPDEFINES = ["WIN32"],
               CPPPATH = [apachedir+"/include"],
               LIBPATH = [apachedir+"/lib"],
               LIBS = ["libapr-1.lib", "libaprutil-1.lib", "libhttpd.lib"],
               SHLINKCOM=["mt.exe -nologo -manifest ${TARGET}.manifest -outputresource:$TARGET;2"])
    env.SideEffect(["mod_websocket.so.manifest", "mod_websocket.exp", "mod_websocket.lib"], "mod_websocket.so")
    env.SideEffect(["mod_websocket_draft76.so.manifest", "mod_websocket_draft76.exp", "mod_websocket_draft76.lib"], "mod_websocket_draft76.so")
    env.SideEffect(["mod_websocket_tcp_proxy.so.manifest", "mod_websocket_tcp_proxy.exp", "mod_websocket_tcp_proxy.lib"], "mod_websocket_tcp_proxy.so")
    env.SideEffect(["mod_websocket_vnc_proxy.so.manifest", "mod_websocket_vnc_proxy.exp", "mod_websocket_vnc_proxy.lib"], "mod_websocket_vnc_proxy.so")

    env["no_import_lib"] = "true"

    modulesdir = apachedir+"/modules"
else:
    env.Append(CCFLAGS = ["-Wall", "-pipe"])
    if int(debug):
        env.Append(CCFLAGS = ["-g"])
    else:
        env.Append(CCFLAGS = ["-O2"])
    if env["PLATFORM"] == "darwin":
        env.Append(CPPDEFINES = ["DARWIN", "SIGPROCMASK_SETS_THREAD_MASK"],
                   CPPPATH = ["/usr/include/apache2", "/usr/include/apr-1"],
                   SHLINKFLAGS = "-undefined dynamic_lookup")
        modulesdir = ARGUMENTS.get('modulesdir', "/usr/libexec/apache2")
    else:
        env.Append(CPPDEFINES = ["LINUX=2", "_FORTIFY_SOURCE=2", "_GNU_SOURCE", "_LARGEFILE64_SOURCE", "_REENTRANT"],
                   CPPPATH = ["/usr/include/apache2", "/usr/include/apr-1.0"])
        modulesdir = ARGUMENTS.get('modulesdir', "/usr/lib/apache2/modules")

mod_websocket = env.SharedLibrary(source=["mod_websocket.c"],
                                  SHLIBPREFIX="",
                                  SHLIBSUFFIX=".so")
mod_websocket_draft76 = env.SharedLibrary(source=["mod_websocket_draft76.c"],
                                  SHLIBPREFIX="",
                                  SHLIBSUFFIX=".so")
mod_websocket_tcp_proxy = env.SharedLibrary(source=["mod_websocket_tcp_proxy.c"],
                                  SHLIBPREFIX="",
                                  SHLIBSUFFIX=".so")
mod_websocket_vnc_proxy = env.SharedLibrary(source=["mod_websocket_vnc_proxy.c"],
                                  SHLIBPREFIX="",
                                  SHLIBSUFFIX=".so")

modulesdir = ARGUMENTS.get('INSTALL_PREFIX', '') + modulesdir

env.Install(dir=modulesdir, source=[mod_websocket, mod_websocket_draft76, mod_websocket_tcp_proxy, mod_websocket_vnc_proxy])

# Install

env.Alias("install", modulesdir)
