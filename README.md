# kbuffer
kbuffer is a general buffer written by c language.

## Examples

	#include <stdio.h>
	#include <assert.h>
	#include <memory.h>
	#include "kbuffer.h"

	int main(int argc, char **argv) {

		// # test case 1
		{
			kbuffer *buf = kbuffer_new();
			size_t i = 0;
			for(i = 0; i < 10; i++)
				kbuffer_add(buf, "data", 4);
			assert(kbuffer_get_size(buf) == 4 * 10);
			kbuffer_free(buf);
		}

		// # test case 2
		{
			kbuffer *buf = kbuffer_new();
			kbuffer_add(buf, "abc", 3);
			kbuffer_add(buf, "def", 3);

			int len = 0;
			const char *str;
			str = (const char *)kbuffer_get_contiguous_data(buf, &len);
			assert(strncmp(str, "abc", 3) == 0 && len == 3);

			kbuffer_drain(buf, 3);
			str = (const char *)kbuffer_get_contiguous_data(buf, &len);
			assert(strncmp(str, "def", 3) == 0 && len == 3);
			kbuffer_free(buf);
		}

		// # test case 3
		{
			kbuffer *buf = kbuffer_new();
			kbuffer_add_printf(buf, "abc%d", 3);

			int len = 0;
			const char *str;
			str = (const char *)kbuffer_get_contiguous_data(buf, &len);
			assert(strncmp(str, "abc3", 4) == 0 && len == 4);
			kbuffer_free(buf);
		}

		printf("unit test ok!\n");
		return 0;
	}

