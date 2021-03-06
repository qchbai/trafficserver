# lib Makefile.am
#
#  Licensed to the Apache Software Foundation (ASF) under one
#  or more contributor license agreements.  See the NOTICE file
#  distributed with this work for additional information
#  regarding copyright ownership.  The ASF licenses this file
#  to you under the Apache License, Version 2.0 (the
#  "License"); you may not use this file except in compliance
#  with the License.  You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
#  Unless required by applicable law or agreed to in writing, software
#  distributed under the License is distributed on an "AS IS" BASIS,
#  WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
#  See the License for the specific language governing permissions and
#  limitations under the License.

SUBDIRS = ts records

if BUILD_PERL_LIB
SUBDIRS += perl
endif

# TsConfig is only built for WCCP until the bugs are worked out.
if BUILD_WCCP
SUBDIRS += tsconfig wccp
endif

if ENABLE_CPPAPI
SUBDIRS += atscppapi
endif

# Some special hacks around building the luajit.
#
#  - Copy the luajit source tree if we're doing out-of-source builds
#  - Upon clean, remove the luajit tree from the out-of-source build tree
#
# ToDo: Can we do this without copying the source? I tried setting the VPATH, but
#       it did not work. Can we make more intelligent build targets, avoiding [ test ] ?
all-local:
	@echo "Making all in luajit"
	test -d "$(top_srcdir)/luajit/src" || (cd "$(top_srcdir)" && git submodule update --init)
	test -d "$(top_builddir)/$(subdir)/luajit/src" || cp -rf "$(srcdir)/luajit" "$(top_builddir)/$(subdir)/"
	cd luajit && make PREFIX=$(prefix)

clean-local:
	test "$(top_srcdir)" != "$(top_builddir)" || (cd "$(top_builddir)/$(subdir)/luajit" && make clean)
	test "$(top_srcdir)" = "$(top_builddir)" || rm -rf "$(top_builddir)/$(subdir)/luajit"
