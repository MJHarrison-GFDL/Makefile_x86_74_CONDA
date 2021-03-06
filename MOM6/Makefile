SHELL = bash -f
PLATFORM = linux
COMPILER = gnu
RESULTS = .results/$(PLATFORM)/$(COMPILER)/repro
all: test_solo

env:
	echo $(CONDA_PREFIX)
	mkdir -p $(RESULTS);\


compile_libs: env
	mkdir -p build/shared.$(PLATFORM).$(COMPILER).debug
	(cd build/shared.$(PLATFORM).$(COMPILER).debug/; \
	rm -f path_names; ../../src/mkmf/bin/list_paths ../../src/FMS; \
	../../src/mkmf/bin/mkmf -t ../../src/mkmf/templates/$(PLATFORM)-$(COMPILER).mk -p libfms.a -o '-I$(CONDA_PREFIX)/include' -c "-Duse_libMPI -Duse_netCDF -DSPMD -DLAND_BND_TRACERS" path_names; \
	source ../../build/env/$(PLATFORM)-$(COMPILER);make  NETCDF=3 DEBUG=1 libfms.a)

	mkdir -p build/shared.$(PLATFORM).$(COMPILER).repro
	(cd build/shared.$(PLATFORM).$(COMPILER).repro/; \
	rm -f path_names; ../../src/mkmf/bin/list_paths ../../src/FMS; \
	../../src/mkmf/bin/mkmf -t ../../src/mkmf/templates/$(PLATFORM)-$(COMPILER).mk -p libfms.a -o '-I$(CONDA_PREFIX)/include' -c "-Duse_libMPI -Duse_netCDF -DSPMD -DLAND_BND_TRACERS" path_names; \
	source ../../build/env/$(PLATFORM)-$(COMPILER); make  NETCDF=3 REPRO=1 libfms.a)

	mkdir -p build/MOM6.$(PLATFORM).$(COMPILER).debug
	(cd build/MOM6.$(PLATFORM).$(COMPILER).debug/; \
	rm -f path_names; ../../src/mkmf/bin/list_paths ../../src/MOM6/{config_src/dynamic_symmetric,src/*,src/*/*,pkg/*/src/*,../../src/SIS2/{config_src/dynamic_symmetric,src/*},../../src/icebergs/*,../../src/ice_ocean_extras/{ice_param,diag_integral}}   ;\
	mv path_names path_names.orig; egrep -v 'combined_ice_ocean_driver' path_names.orig > path_names; \
	../../src/mkmf/bin/mkmf -t ../../src/mkmf/templates/$(PLATFORM)-$(COMPILER).mk -o '-I$(CONDA_PREFIX)/include -I../../src/FMS/include -I../shared.$(PLATFORM).$(COMPILER).debug' -p libMOM6.a -l '-L../shared.$(PLATFORM).$(COMPILER).debug -lfms' -c "-Duse_libMPI -Duse_netCDF -DSPMD -DLAND_BND_TRACERS -Duse_AM3_physics -DSYMMETRIC_MEMORY_" path_names; \
	source ../../build/env/$(PLATFORM)-$(COMPILER); make  NETCDF=3 DEBUG=1 -j 8 libMOM6.a)

	mkdir -p build/MOM6.$(PLATFORM).$(COMPILER).repro
	(cd build/MOM6.$(PLATFORM).$(COMPILER).repro/; \
	rm -f path_names; ../../src/mkmf/bin/list_paths ../../src/MOM6/{config_src/dynamic_symmetric,src/*,src/*/*,pkg/*/src/*,../../src/SIS2/{config_src/dynamic_symmetric,src/*},../../src/icebergs/*,../../src/ice_ocean_extras/{ice_param,diag_integral}}   ;\
	mv path_names path_names.orig; egrep -v 'combined_ice_ocean_driver' path_names.orig > path_names; \
	../../src/mkmf/bin/mkmf -t ../../src/mkmf/templates/$(PLATFORM)-$(COMPILER).mk -o '-I$(CONDA_PREFIX)/include -I../../src/FMS/include -I../shared.$(PLATFORM).$(COMPILER).repro' -p libMOM6.a -l '-L../shared.$(PLATFORM).$(COMPILER).repro -lfms' -c "-Duse_libMPI -Duse_netCDF -DSPMD -DLAND_BND_TRACERS -Duse_AM3_physics -DSYMMETRIC_MEMORY_" path_names; \
	source ../../build/env/$(PLATFORM)-$(COMPILER); make  NETCDF=3 REPRO=1 -j 8 libMOM6.a)


compile_MOM6_solo: compile_libs
	mkdir -p build/MOM6_solo.$(PLATFORM).$(COMPILER).debug
	(cd build/MOM6_solo.$(PLATFORM).$(COMPILER).debug/; \
	rm -f path_names; ../../src/mkmf/bin/list_paths ../../src/MOM6/config_src/solo_driver/*  ;\
        mv path_names path_names.orig; egrep -v 'coupler_types.F90' path_names.orig > path_names; \
	../../src/mkmf/bin/mkmf -t ../../src/mkmf/templates/$(PLATFORM)-$(COMPILER).mk -o  '-I../../src/MOM6/src/framework -I../../src/MOM6/config_src/dynamic_symmetric -I../MOM6.$(PLATFORM).$(COMPILER).debug -I../shared.$(PLATFORM).$(COMPILER).debug' -l '-L../MOM6.$(PLATFORM).$(COMPILER).debug -lMOM6 -L../shared.$(PLATFORM).$(COMPILER).debug -lfms -L$(CONDA_PREFIX)/lib -lmpich -lmpichf90 -lnetcdf -lnetcdff -lmpl -lmfhdf -ldf -lhdf5_hl -lhdf5 -ljpeg' -p MOM6 -c "-Duse_libMPI -Duse_netCDF -DSPMD -DLAND_BND_TRACERS -Duse_AM3_physics" path_names; \
	source ../../build/env/$(PLATFORM)-$(COMPILER); make  NETCDF=3 DEBUG=1 -j 8 MOM6)

	mkdir -p build/MOM6_solo.$(PLATFORM).$(COMPILER).repro
	(cd build/MOM6_solo.$(PLATFORM).$(COMPILER).repro/; \
	rm -f path_names; ../../src/mkmf/bin/list_paths ../../src/MOM6/config_src/solo_driver/*  ;\
        mv path_names path_names.orig; egrep -v 'coupler_types.F90' path_names.orig > path_names; \
	../../src/mkmf/bin/mkmf -t ../../src/mkmf/templates/$(PLATFORM)-$(COMPILER).mk -o  '-I../../src/MOM6/src/framework -I../../src/MOM6/config_src/dynamic_symmetric -I../MOM6.$(PLATFORM).$(COMPILER).repro -I../shared.$(PLATFORM).$(COMPILER).repro' -l '-L../MOM6.$(PLATFORM).$(COMPILER).repro -lMOM6 -L../shared.$(PLATFORM).$(COMPILER).repro -lfms -L$(CONDA_PREFIX)/lib -lmpich -lmpichf90 -lnetcdf -lnetcdff -lmpl  -lmfhdf -ldf -lhdf5_hl -lhdf5 -ljpeg' -p MOM6 -c "-Duse_libMPI -Duse_netCDF -DSPMD -DLAND_BND_TRACERS -Duse_AM3_physics" path_names; \
	source ../../build/env/$(PLATFORM)-$(COMPILER); make  NETCDF=3 REPRO=1 -j 8 MOM6)


compile_MOM6_SIS2: compile_libs
	mkdir -p build/MOM6_SIS2.$(PLATFORM).$(COMPILER).debug
	(cd build/MOM6_SIS2.$(PLATFORM).$(COMPILER).debug/; \
	rm -f path_names; ../../src/mkmf/bin/list_paths  ../../src/MOM6/config_src/coupled_driver/* ../../src/land_null/* ../../src/ice_ocean_extras/*/* ../../src/atmos_null/* ../../src/coupler/*;\
	../../src/mkmf/bin/mkmf -t ../../src/mkmf/templates/$(PLATFORM)-$(COMPILER).mk -o  '-I../../src/FMS/include -I../../src/MOM6/src/framework -I../../src/MOM6/config_src/dynamic_symmetric -I../MOM6.$(PLATFORM).$(COMPILER).debug  -I../shared.$(PLATFORM).$(COMPILER).debug' -l '-L../MOM6.$(PLATFORM).$(COMPILER).debug -lMOM6 -L../shared.$(PLATFORM).$(COMPILER).debug -lfms ' -p MOM6_SIS2 -c "-Duse_libMPI -Duse_netCDF -DSPMD -DLAND_BND_TRACERS -Duse_AM3_physics -D_USE_LEGACY_LAND_" path_names; \
	source ../../build/env/$(PLATFORM)-$(COMPILER); make  NETCDF=3 DEBUG=1 -j 8 MOM6_SIS2)

	mkdir -p build/MOM6_SIS2.$(PLATFORM).$(COMPILER).repro
	(cd build/MOM6_SIS2.$(PLATFORM).$(COMPILER).repro/; \
	rm -f path_names; ../../src/mkmf/bin/list_paths  ../../src/MOM6/config_src/coupled_driver/* ../../src/land_null/* ../../src/ice_ocean_extras/*/* ../../src/atmos_null/* ../../src/coupler/*;\
	../../src/mkmf/bin/mkmf -t ../../src/mkmf/templates/$(PLATFORM)-$(COMPILER).mk -o  '-I../../src/FMS/include -I../../src/MOM6/src/framework -I../../src/MOM6/config_src/dynamic_symmetric -I../MOM6.$(PLATFORM).$(COMPILER).repro  -I../shared.$(PLATFORM).$(COMPILER).repro' -l '-L../MOM6.$(PLATFORM).$(COMPILER).repro -lMOM6 -L../shared.$(PLATFORM).$(COMPILER).repro -lfms ' -p MOM6_SIS2 -c "-Duse_libMPI -Duse_netCDF -DSPMD -DLAND_BND_TRACERS -Duse_AM3_physics -D_USE_LEGACY_LAND_" path_names; \
	source ../../build/env/$(PLATFORM)-$(COMPILER); make  NETCDF=3 REPRO=1 -j 8 MOM6_SIS2)


test_solo: compile_MOM6_solo
#	(export LD_LIBRARY_PATH=$(CONDA_PREFIX)/lib;cd ocean_only/circle_obcs;mpirun -n 1 ../../build/MOM6_solo.$(PLATFORM).$(COMPILER).repro/MOM6 >&output; echo "circle_obcs 1pe :" | tee  ../../$(RESULTS)/results;diff -s ocean.stats ../../regressions/ocean_only/circle_obcs/ocean.stats.$(COMPILER) | tee -a ../../$(RESULTS)/results)
#	(export LD_LIBRARY_PATH=$(CONDA_PREFIX)/lib;cd ocean_only/circle_obcs;mpirun -n 4 ../../build/MOM6_solo.$(PLATFORM).$(COMPILER).repro/MOM6 >&output; echo "circle_obcs 4pe :" | tee -a  ../../$(RESULTS)/results;diff -s ocean.stats ../../regressions/ocean_only/circle_obcs/ocean.stats.$(COMPILER) | tee -a ../../$(RESULTS)/results)
#	(export LD_LIBRARY_PATH=$(CONDA_PREFIX)/lib;cd ocean_only/Channel;mpirun -n 1 ../../build/MOM6_solo.$(PLATFORM).$(COMPILER).repro/MOM6 >&output; echo "Channel 1pe :" | tee -a  ../../$(RESULTS)/results;diff -s ocean.stats ../../regressions/ocean_only/Channel/ocean.stats.$(COMPILER) | tee -a ../../$(RESULTS)/results)
#	(export LD_LIBRARY_PATH=$(CONDA_PREFIX)/lib;cd ocean_only/Channel;mpirun -n 4 ../../build/MOM6_solo.$(PLATFORM).$(COMPILER).repro/MOM6 >&output; echo "Channel 4pe :" | tee -a  ../../$(RESULTS)/results;diff -s ocean.stats ../../regressions/ocean_only/Channel/ocean.stats.$(COMPILER) | tee -a ../../$(RESULTS)/results)
#	(export LD_LIBRARY_PATH=$(CONDA_PREFIX)/lib;cd ocean_only/seamount/z;mpirun -n 4 ../../../build/MOM6_solo.$(PLATFORM).$(COMPILER).repro/MOM6 >&output; echo "Seamount 4pe :" | tee -a  ../../../$(RESULTS)/results;diff -s ocean.stats ../../../regressions/ocean_only/seamount/z/ocean.stats.$(COMPILER) | tee -a ../../../$(RESULTS)/results)
#	(export LD_LIBRARY_PATH=$(CONDA_PREFIX)/lib;cd ocean_only/DOME;mpirun -n 4 ../../build/MOM6_solo.$(PLATFORM).$(COMPILER).repro/MOM6 >&output; echo "DOME 4pe :" | tee -a  ../../$(RESULTS)/results;diff -s ocean.stats ../../regressions/ocean_only/DOME/ocean.stats.$(COMPILER) | tee -a ../../$(RESULTS)/results)
#	(export LD_LIBRARY_PATH=$(CONDA_PREFIX)/lib;cd ocean_only/Tidal_bay;mpirun -n 4 ../../build/MOM6_solo.$(PLATFORM).$(COMPILER).repro/MOM6 >&output; echo "Tidal Bay 4pe :" | tee -a  ../../$(RESULTS)/results;diff -s ocean.stats ../../regressions/ocean_only/Tidal_bay/ocean.stats.$(COMPILER) | tee -a ../../$(RESULTS)/results)
	(export LD_LIBRARY_PATH=$(CONDA_PREFIX)/lib;cd ocean_only/dumbbell/z;mpirun -n 1 ../../../build/MOM6_solo.$(PLATFORM).$(COMPILER).debug/MOM6 >&output; echo "Dumbbell z 4pe :" | tee -a  ../../../$(RESULTS)/results;diff -s ocean.stats ../../../regressions/ocean_only/dumbbell/z/ocean.stats.$(COMPILER) | tee -a ../../../$(RESULTS)/results)
#	(export LD_LIBRARY_PATH=$(CONDA_PREFIX)/lib;source build/env/$(PLATFORM);cd ocean_only/dumbbell/z_sub;mpirun -n 4 ../../../build/MOM6_solo.$(PLATFORM).$(COMPILER).debug/MOM6 >&output; echo "Dumbbell z_sub 4pe :" | tee -a  ../../../$(RESULTS)/results;diff -s ocean.stats ../../../regressions/ocean_only/dumbbell/z_sub/ocean.stats.$(COMPILER) | tee -a ../../../$(RESULTS)/results)


test_solo_restart: compile_MOM6_solo
#		   (export LD_LIBRARY_PATH=$(CONDA_PREFIX)/lib;cd ocean_only/circle_obcs;mkdir RESTART;sed -i 's/hours = 0/hours = 1/g' input.nml;mpirun -n 1 ../../build/MOM6_solo.$(PLATFORM).$(COMPILER).repro/MOM6 >&output;mv ocean.stats ocean.stats.2x1hr;mv RESTART/{MOM.res.nc,ocean_solo.res} .;sed -i "s/input_filename = 'n'/input_filename = 'r'/g" input.nml;mpirun -n 1 ../../build/MOM6_solo.$(PLATFORM).$(COMPILER).repro/MOM6 >&outputA;cat ocean.stats >> ocean.stats.2x1hr;sed -i "s/input_filename = 'r'/input_filename = 'n'/g" input.nml;sed -i 's/hours = 1/hours = 0/g' input.nml;rm MOM.res.nc ocean_solo.res;mpirun -n 1 ../../build/MOM6_solo.$(PLATFORM).$(COMPILER).repro/MOM6 >&outputB;mv ocean.stats ocean.stats.2hr;echo "Circle OBCs Restart check:" | tee -a ../../$(RESULTS)/results;diff -s ocean.stats.2x1hr ocean.stats.2hr | tee -a ../../$(RESULTS)/results;rm -rf RESTART)

#		    (export LD_LIBRARY_PATH=$(CONDA_PREFIX)/lib;cd ocean_only/Channel;mkdir RESTART;sed -i 's/seconds = 0/seconds = 50/g' input.nml;mpirun -n 4 ../../build/MOM6_solo.$(PLATFORM).$(COMPILER).repro/MOM6 >&output;mv ocean.stats ocean.stats.2x50s;mv RESTART/{MOM.res.nc,ocean_solo.res} .;sed -i "s/input_filename = 'n'/input_filename = 'r'/g" input.nml;mpirun -n 4 ../../build/MOM6_solo.$(PLATFORM).$(COMPILER).repro/MOM6 >&outputA;cat ocean.stats >> ocean.stats.2x50s;sed -i "s/input_filename = 'r'/input_filename = 'n'/g" input.nml;sed -i 's/seconds = 50/seconds = 0/g' input.nml;rm MOM.res.nc ocean_solo.res;mpirun -n 4 ../../build/MOM6_solo.$(PLATFORM).$(COMPILER).repro/MOM6 >&outputB;mv ocean.stats ocean.stats.100s;echo "Channel Restart check:" | tee -a ../../$(RESULTS)/results;diff -s ocean.stats.2x50s ocean.stats.100s | tee -a ../../$(RESULTS)/results;rm -rf RESTART)

		    (export LD_LIBRARY_PATH=$(CONDA_PREFIX)/lib;cd ocean_only/Tidal_bay;mkdir RESTART;sed -i 's/days = 0/days = 1/g' input.nml;mpirun -n 4 ../../build/MOM6_solo.$(PLATFORM).$(COMPILER).repro/MOM6 >&output;mv ocean.stats ocean.stats.2x1d;mv RESTART/{MOM.res.nc,ocean_solo.res} .;sed -i "s/input_filename = 'n'/input_filename = 'r'/g" input.nml;mpirun -n 4 ../../build/MOM6_solo.$(PLATFORM).$(COMPILER).repro/MOM6 >&outputA;cat ocean.stats >> ocean.stats.2x1d;sed -i "s/input_filename = 'r'/input_filename = 'n'/g" input.nml;sed -i 's/days = 1/days = 0/g' input.nml;rm MOM.res.nc ocean_solo.res;mpirun -n 4 ../../build/MOM6_solo.$(PLATFORM).$(COMPILER).repro/MOM6 >&outputB;mv ocean.stats ocean.stats.2d;echo "Tidal_bay restart check:" | tee -a ../../$(RESULTS)/results;diff -s ocean.stats.2x1d ocean.stats.2d | tee -a ../../$(RESULTS)/results;rm -rf RESTART)

#		    (export LD_LIBRARY_PATH=$(CONDA_PREFIX)/lib;cd ocean_only/DOME;mkdir RESTART;sed -i 's/days = 0/days = 1/g' input.nml;mpirun -n 4 ../../build/MOM6_solo.$(PLATFORM).$(COMPILER).repro/MOM6 >&output;mv ocean.stats ocean.stats.2x1d;mv RESTART/{MOM.res.nc,ocean_solo.res} .;sed -i "s/input_filename = 'n'/input_filename = 'r'/g" input.nml;mpirun -n 4 ../../build/MOM6_solo.$(PLATFORM).$(COMPILER).repro/MOM6 >&outputA;cat ocean.stats >> ocean.stats.2x1d;sed -i "s/input_filename = 'r'/input_filename = 'n'/g" input.nml;sed -i 's/days = 1/days = 0/g' input.nml;rm MOM.res.nc ocean_solo.res;mpirun -n 4 ../../build/MOM6_solo.$(PLATFORM).$(COMPILER).repro/MOM6 >&outputB;mv ocean.stats ocean.stats.2d;echo "DOME restart check:" | tee -a ../../$(RESULTS)/results;diff -s ocean.stats.2x1d ocean.stats.2d | tee -a ../../$(RESULTS)/results;rm -rf RESTART)

clean:
	(rm -rf build/{MOM6_SIS2.$(PLATFORM).$(COMPILER).debug,MOM6_solo.$(PLATFORM).$(COMPILER).debug,MOM6.$(PLATFORM).$(COMPILER).debug,shared.$(PLATFORM).$(COMPILER).debug})
	(rm -rf build/{MOM6_SIS2.$(PLATFORM).$(COMPILER).repro,MOM6_solo.$(PLATFORM).$(COMPILER).repro,MOM6.$(PLATFORM).$(COMPILER).repro,shared.$(PLATFORM).$(COMPILER).repro})
