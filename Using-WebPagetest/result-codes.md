# 结果代码
结果代码为0（成功）或99999（内容错误）是成功的测试，所有其他结果都是错误。

| 结果代码 | 描述  |
| :------------ |:---------------|
| 0      | some wordy text |
| 4xx-5xx      | centered        |
| 99996 | are neat        |
| 99997 | are neat        |
| 99998 | are neat        |
| 99999 | are neat        |

0	Successful Test
4xx-5xx	HTTP Result (Base Page Error)
99996	Test Failed waiting for specified DOM element/End condition
99997	Test Timed Out (no content errors)
99998	Test Timed Out (content errors)
99999	Test Completed successfully but individual requests failed (content errors)