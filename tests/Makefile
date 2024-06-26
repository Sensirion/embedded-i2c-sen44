driver_dir := ..
mux_dir := ../i2c-mux-testbed

i2c_mux_sources = ${mux_dir}/i2c_mux.h ${mux_dir}/i2c_mux.c
common_sources = ${driver_dir}/sensirion_config.h ${driver_dir}/sensirion_common.h ${driver_dir}/sensirion_common.c
i2c_sources = ${driver_dir}/sensirion_i2c_hal.h ${driver_dir}/sensirion_i2c.h ${driver_dir}/sensirion_i2c.c
sensirion_test_sources = sensirion_test_setup.cpp ${i2c_mux_sources}

sw_i2c_dir := ${driver_dir}/sample-implementations/GPIO_bit_banging

hw_i2c_impl_src = ${driver_dir}/sample-implementations/linux_user_space/sensirion_i2c_hal.c
sw_i2c_impl_src = ${sw_i2c_dir}/sample-implementations/linux_user_space/sensirion_i2c_gpio.c ${sw_i2c_dir}/sensirion_i2c_hal.c

sen44_sources = ${driver_dir}/sen44_i2c.h ${driver_dir}/sen44_i2c.c

CXXFLAGS ?= $(CFLAGS) -fsanitize=address -I${mux_dir} -I${driver_dir} -I${sw_i2c_dir}
ifdef CI
	CXXFLAGS += -Werror
endif
LDFLAGS ?= -lasan -lstdc++ -lCppUTest -lCppUTestExt

.PHONY: clean test


sen44_test_binaries := sen44-test-hw_i2c sen44-test-sw_i2c

all: ${sen44_test_binaries}

sen44-test-hw_i2c: sen44_i2c_test.cpp ${sen44_sources} ${sensirion_test_sources} ${i2c_sources} ${hw_i2c_impl_src} ${common_sources}
	$(CXX) $(CXXFLAGS) -o $@ $^ $(LDFLAGS)

sen44-test-sw_i2c: sen44_i2c_test.cpp ${sen44_sources} ${sensirion_test_sources} ${i2c_sources} ${sw_i2c_impl_src} ${common_sources}
	$(CXX) $(CXXFLAGS) -o $@ $^ $(LDFLAGS)

test: ${sen44_test_binaries}
	set -ex; for test in ${sen44_test_binaries}; do echo $${test}; ./$${test}; echo; done;

clean:
	$(RM) ${sen44_test_binaries}
