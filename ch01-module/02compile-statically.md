# 1.2ģ�龲̬����

�ܵ������ǣ�
	����׶Σ������ļ����� __initcall ����module_init ��ģ��� xxx_module_init������ __initcall ����
	���н׶Σ�start_kernel������������� __initcall ����� xxx_module_init��


```

```

```
#define module_init(x)  __initcall(x);
	#define __initcall(fn) device_initcall(fn)
		#define device_initcall(fn)     __define_initcall(fn, 6)
			#define __define_initcall(fn, id) \
                static initcall_t __initcall_##fn##id __used \
                __attribute__((__section__(".initcall" #id ".init"))) = fn	<--module��̬����ʱ��xxx_mod_init���������ý�initCall����������
*/
```

```
//sfw**module**�ں˳�ʼ��������
start_kernel
	rest_init
    	kernel_thread
        	kernel_init
            	kernel_init_freeable
                	do_basic_setup				<------------
                    	do_initcalls
                        	do_initcall_level(level)
                            	do_one_initcall(initcall_t fn)	<--module��̬����ʱ��xxx_mod_init���������ã�������
```



## 1.1.2ģ�鶯̬����

�ܵ������ǣ�
	����׶Σ���������.ko�ļ���
	���н׶Σ�ͨ�� insmod ϵͳ���ã����ں˿ռ����xxx_mod_init()������

```
//sfw**module**module_init����(ģ�鶯̬����)
#define module_init(initfn)					\
//sfw**__inittest��������initfn������Ϊinitcall_t
	static inline initcall_t __maybe_unused __inittest(void)		\
	{ return initfn; }					\
//sfw**initfn������������Ϊinit_module
	int init_module(void) __attribute__((alias(#initfn)));	
```

