# This particular test has to be put in a separate directory, otherwise, for a reason
# which I cannot figure out, CMake will pass multiple times the "-D"s relative to the
# cling workarounds to genreflex. Given that other tests check some properties of the
# fwd declaration payload in the dictionaries, this "feature" makes them fail.
# In a sense, this is a workaround to have a sane reproducer of ROOT-9660 which does 
# not interfere with the other tests.

# ROOT-9660
ROOTTEST_GENERATE_REFLEX_DICTIONARY(checkEnumFwdDeclxDict x.h SELECTION selection_x.xml)
ROOTTEST_GENERATE_REFLEX_DICTIONARY(checkEnumFwdDeclyDict y.h SELECTION selection_y.xml)

if(ROOT_pyroot_FOUND)
  ROOTTEST_ADD_TEST(checkEnumFwdDecl
                    MACRO checkEnumFwdDecl.py
                    OUTREF empty.ref
                    ERRREF empty.ref
                    DEPENDS checkEnumFwdDeclyDict-libgen-build checkEnumFwdDeclxDict-libgen-build)
endif()

ROOTTEST_ADD_TESTDIRS()

