TOP = ..
include $(TOP)/configure/CONFIG
include $(TOP)/configure/CONFIG_PY

# place .so in subdirectory
INSTALL_SHRLIB = $(PY_INSTALL_DIR)/p4p

LOADABLE_LIBRARY_HOST += _p4p

_p4p_SRCS += p4p_top.cpp
_p4p_SRCS += p4p_type.cpp
_p4p_SRCS += p4p_value.cpp
_p4p_SRCS += p4p_array.cpp
_p4p_SRCS += p4p_server.cpp
_p4p_SRCS += p4p_server_provider.cpp
_p4p_SRCS += p4p_server_sharedpv.cpp

_p4p_SRCS += p4p_client.cpp

_p4p_LIBS += pvAccess pvData Com

LOADABLE_LIBRARY_HOST += _gw

_gw_SRCS += _gw.cpp
_gw_SRCS += gwchannel.cpp

_gw_LIBS += pvAccess pvData Com

PY += p4p/__init__.py
PY += p4p/disect.py
PY += p4p/wrapper.py
PY += p4p/rpc.py
PY += p4p/util.py
PY += p4p/server/__init__.py
PY += p4p/server/raw.py
PY += p4p/server/thread.py
PY += p4p/server/cothread.py
PY += p4p/server/asyncio.py
PY += p4p/server/cli.py
PY += p4p/nt/__init__.py
PY += p4p/nt/common.py
PY += p4p/nt/scalar.py
PY += p4p/nt/enum.py
PY += p4p/nt/ndarray.py
PY += p4p/client/__init__.py
PY += p4p/client/cli.py
PY += p4p/client/raw.py
PY += p4p/client/thread.py
PY += p4p/client/asyncio.py
PY += p4p/client/cothread.py
PY += p4p/client/Qt.py

PY += p4p/gw.py
PY += p4p/asLib/__init__.py
PY += p4p/asLib/lex.py
PY += p4p/asLib/yacc.py
PY += p4p/asLib/pvlist.py

PY += p4p/test/__init__.py
PY += p4p/test/utils.py
PY += p4p/test/test_type.py
PY += p4p/test/test_value.py
PY += p4p/test/test_client_raw.py
PY += p4p/test/test_client_thread.py
PY += p4p/test/test_server.py
PY += p4p/test/test_rpc.py
PY += p4p/test/test_nt.py
PY += p4p/test/test_sharedpv.py
PY += p4p/test/test_cothread.py
PY += p4p/test/test_gw.py
PY += p4p/test/test_asLib.py

PY += p4p/test/test_asyncio.py
PY += p4p/test/asynciotest.py

PERL_SCRIPTS += pvagw

include $(TOP)/configure/RULES
include $(TOP)/configure/RULES_PY

pvagw: ../bootgw.py
	$(PYTHON) $< -P "../../python$(PY_LD_VER)/$(EPICS_HOST_ARCH)" $@

_gw.cpp: ../p4p/_gw.pyx
	$(PYTHON) -m cython -o $@ --cplus $<
#	$(PYTHON) -c 'from Cython.Compiler.Main import main; main(command_line = 1)' -o $@ --cplus $<
_gw.h: _gw.cpp ../p4p/_gw.pyx

gwchannel$(OBJ): _gw.h
gwchannel$(DEP): _gw.h

ifneq (,$(T_A))
nose.%:
	PYTHONPATH="${PYTHONPATH}:$(abspath $(TOP))/python$(PY_LD_VER)/$(T_A)" $(PYTHON) -m nose -P -v $* $(NOSEFLAGS)

# bounce back down to the sphinx generated Makefile
# aren't Makefiles fun...
sphinx:
	PYTHONPATH="${PYTHONPATH}:$(abspath $(TOP))/python$(PY_LD_VER)/$(T_A)" $(MAKE) -C $(TOP)/documentation html

sh:
	echo "export PYTHONPATH=\$${PYTHONPATH}:$(abspath $(TOP))/python$(PY_LD_VER)/$(T_A)" > $(OUTPUT)

ipython:
	PYTHONPATH="${PYTHONPATH}:$(abspath $(TOP))/python$(PY_LD_VER)/$(T_A)" $(PYTHON) -c "import sys; sys.argv[0] = '$(PYTHON)'; from IPython.terminal.ipapp import launch_new_instance; launch_new_instance()"

endif

.PHONY: nose sphinx sh
