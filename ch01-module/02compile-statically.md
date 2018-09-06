# 1.2ģ�龲̬����

�ܵ������ǣ�  
-    ����׶Σ������ļ����� __initcall ����module_init ��ģ��� xxx_module_init������ __initcall ����  
-    ���н׶Σ�start_kernel������������� __initcall ����� xxx_module_init��  


linux\include\linux\export.h
```
#define __VMLINUX_SYMBOL(x) _##x
#define VMLINUX_SYMBOL(x) __VMLINUX_SYMBOL(x)				
````
���� __VMLINUX_SYMBOL(x) ���� x������ VMLINUX_SYMBOL(x) ���� x��

linux\include\asm-generic\vmlinux.lds.h
```	
#define INIT_CALLS_LEVEL(level)						\
		VMLINUX_SYMBOL(__initcall##level##_start) = .;		\
		KEEP(*(.initcall##level##.init))			\
		KEEP(*(.initcall##level##s.init))			\
```
����INIT_CALLS_LEVEL(x)���ڣ�
```
		__initcall_x_start) = .;
		KEEP(*(.initcallx.init))	
		KEEP(*(.initcallxs.init))
```

```
#define INIT_CALLS							\
		VMLINUX_SYMBOL(__initcall_start) = .;			\
		KEEP(*(.initcallearly.init))				\
		INIT_CALLS_LEVEL(0)					\
		INIT_CALLS_LEVEL(1)					\
		INIT_CALLS_LEVEL(2)					\
		INIT_CALLS_LEVEL(3)					\
		INIT_CALLS_LEVEL(4)					\
		INIT_CALLS_LEVEL(5)					\
		INIT_CALLS_LEVEL(rootfs)				\
		INIT_CALLS_LEVEL(6)					\
		INIT_CALLS_LEVEL(7)					\
		VMLINUX_SYMBOL(__initcall_end) = .;
```
���� INIT_CALLS ���ڣ�
```
		__initcall_start) = .;			
		KEEP(*(.initcallearly.init))					
		__initcall_0_start) = .;
		KEEP(*(.initcall.init))	
		KEEP(*(.initcalls.init))
		__initcall_1_start) = .;
		KEEP(*(.initcall.init))	
		KEEP(*(.initcalls.init))
				.
				.
				.
		__initcall_7_start) = .;
		KEEP(*(.initcall.init))	
		KEEP(*(.initcalls.init))				
		__initcall_end = .;
```

```		
#define INIT_DATA_SECTION(initsetup_align)				\		
	.init.data : AT(ADDR(.init.data) - LOAD_OFFSET) {		\
		INIT_DATA						\
		INIT_SETUP(initsetup_align)				\
		INIT_CALLS						\			
		CON_INITCALL						\
		SECURITY_INITCALL					\
		INIT_RAM_FS						\
	}

```

linux\arch\x86\kernel\vmlinux.lds.S
```
	INIT_DATA_SECTION(16)
```


linux\include\linux\module.h

```
#define module_init(x)  __initcall(x);
	#define __initcall(fn) device_initcall(fn)
		#define device_initcall(fn)     __define_initcall(fn, 6)
			#define __define_initcall(fn, id) \
                static initcall_t __initcall_##fn##id __used \
                __attribute__((__section__(".initcall" #id ".init"))) = fn	
				<--module��̬����ʱ��xxx_mod_init���������ý�initCall����������
*/
```

linux\init\main.c
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