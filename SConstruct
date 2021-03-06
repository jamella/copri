env = Environment(
	LIBPATH = ['.', '/usr/lib', '/usr/local/lib'],
    CPPPATH = ['/usr/include', '/usr/local/include'],
    CCFLAGS = ['-Wall'],
    OMP = 0,
    BUILD_TESTS = 0,
    LIBS = ['gmp', 'copri', 'array']
)


AddOption("--test", action="store_true", dest="test", default=False, help="build tests")
AddOption("--no-omp", action="store_false", dest="omp", default=True, help="don't use OpenMP multithreading")

env['BUILD_TESTS'] = GetOption('test')

if env['PLATFORM'] == 'posix':
	if GetOption('omp'):
		env['OMP'] = 1
elif env['PLATFORM'] == 'darwin':
	env.AppendUnique(CCFLAGS =['-Wno-deprecated-declarations'])
	env['CC'] = 'clang'
else:
	print('Platform %s is not supported' % env['PLATFORM'])

if env['OMP']:
	env.AppendUnique(LINKFLAGS = ['-fopenmp'])
	env.AppendUnique(CCFLAGS =['-fopenmp'])

env.Library('array', ['array.c'], LIBS = ['gmp'])

env.Library('copri', ['copri.c'])

env.Program('gen', ['gen.c'], LIBS = ['array', 'gmp', 'crypto'])

if env['BUILD_TESTS']:

	env.AppendUnique(CCFLAGS =['-g'])
	
	env.Program('test-prod', ['test-prod.c'])

	env.Program('test-array', ['test-array.c'])
	
	env.Program('test-arrayio', ['test-arrayio.c'])
	
	env.Program('test-twopower', ['test-twopower.c'])
	
	env.Program('test-gcdppippo', ['test-gcdppippo.c'])
	
	env.Program('test-gcdppgpple', ['test-gcdppgpple.c'])
	
	env.Program('test-appendcb', ['test-appendcb.c'])
	
	env.Program('test-split', ['test-split.c'])
	
	env.Program('test-cbextend', ['test-cbextend.c'])
	
	env.Program('test-cbmerge', ['test-cbmerge.c'])
	
	env.Program('test-cb', ['test-cb.c'])
	
	env.Program('test-findfactor', ['test-findfactor.c'])
	
env.Program('app', ['app.c'])

env.Program('app-n2', ['app-n2.c'], LIBS = ['array', 'copri', 'gmp'])

def config_h_build(target, source, env):

	config_h_defines = {
		"install_prefix": "/usr/local",
		"version_str": "0.9",
		"openmp": env['OMP']
	}

	for a_target, a_source in zip(target, source):
		config_h = file(str(a_target), "w")
		config_h_in = file(str(a_source), "r")
		config_h.write(config_h_in.read() % config_h_defines)
		config_h_in.close()
		config_h.close()

env.AlwaysBuild(env.Command('config.h', 'config.h.in', config_h_build))