#include <glad/glad.h>//包含OpenGL头文件，需要写在其他依赖于OpenGL的头文件之前
#include <GLFW/glfw3.h>
#include <iostream>

const char* vertexShaderSource = "#version 330 core\n"//顶点着色器的源代码
"layout (location = 0) in vec3 aPos;\n"
"void main()\n"
"{\n"
"   gl_Position = vec4(aPos.x, aPos.y, aPos.z, 1.0);\n"
"}\0";
const char* fragmentShaderSource = "#version 330 core\n"//片段着色器源代码
"out vec4 FragColor;\n"
"void main()\n"
"{\n"
"FragColor = vec4(1.0f, 0.5f, 0.2f, 1.0f);\n"
"}\0";
const char* fragmentShaderSource2 = "#version 330 core\n"//片段着色器源代码
"out vec4 FragColor;\n"
"void main()\n"
"{\n"
"FragColor = vec4(1.0f, 0.2f, 0.5f, 1.0f);\n"
"}\0";
void framebuffer_size_callback(GLFWwindow* window, int width, int height);
void processInput(GLFWwindow* window);
int main()
{
    #pragma region InitializeWindow
	glfwInit();//初始化GLFW，一个专门针对OpenGL的C语言库，提供渲染物体所需最低限度的接口
	//初始化后可调用下面的函数来配置GLFW，告诉GLFW我们使用的OpenGL版本是3.3
	glfwWindowHint(GLFW_CONTEXT_VERSION_MAJOR, 3);//将主版本号设置为3
	glfwWindowHint(GLFW_CONTEXT_VERSION_MINOR, 3);//将次版本号设置为3
	//第一个参数代表选项的名称，我们可以从很多以GLFW_开头的枚举值中选择；
	//第二个参数接受一个整型，用来设置这个选项的值。
	glfwWindowHint(GLFW_OPENGL_PROFILE, GLFW_OPENGL_CORE_PROFILE);
	//告知GLFW我们使用OpenGL的核心模式，意味着我们只能使用OpenGL功能的子集

	GLFWwindow* window = glfwCreateWindow(800, 600, "LearnOpenGL", NULL, NULL);//创建窗口对象，通过函数设置宽，高，名称
	if (window == NULL)
	{
		std::cout << "Failed to create GLFW window" << std::endl;
		glfwTerminate();
		return -1;
	}
	glfwMakeContextCurrent(window);//通知GLFW将窗口的上下文设置为当前线程的主上下文

	if (!gladLoadGLLoader((GLADloadproc)glfwGetProcAddress))//GLAD是用来管理OpenGL的函数指针的，所以在调用任何OpenGL的函数之前我们需要初始化GLAD。
	{
		std::cout << "Failed to initialize GLAD" << std::endl;
		return -1;
	}

	glViewport(0, 0, 800, 600);//告诉OpenGL渲染窗口的尺寸大小，之前的window，应该是一个对象，从抽象层面规定了窗口的信息，便于数据的计算
	//glViewport函数前两个参数控制窗口左下角的位置。第三个和第四个参数控制渲染窗口的宽度和高度（像素）。
	glfwSetFramebufferSizeCallback(window, framebuffer_size_callback);//注册此函数，告诉GLFW我们希望调整窗口大小时调用次函数
#pragma endregion

	#pragma region ShaderCreate
	int  success;//用于检测故障
	char infoLog[512];//用以存储错误信息

	//动态编译顶点着色器源代码
	unsigned int vertexShader;//着色器对象ID，同样用于存储地址
	vertexShader = glCreateShader(GL_VERTEX_SHADER);//创建着色器对象
	glShaderSource(vertexShader, 1, &vertexShaderSource, NULL);//将着色器源码与着色器对象绑定
	glCompileShader(vertexShader);//编译
	//动态编译片段着色器源代码
	unsigned int fragmentShader;
	fragmentShader = glCreateShader(GL_FRAGMENT_SHADER);
	glShaderSource(fragmentShader, 1, &fragmentShaderSource, NULL);
	glCompileShader(fragmentShader);

	unsigned int fragmentShader2;
	fragmentShader2 = glCreateShader(GL_FRAGMENT_SHADER);
	glShaderSource(fragmentShader2, 1, &fragmentShaderSource2, NULL);
	glCompileShader(fragmentShader2);
	//检测是否编译成功
	glGetShaderiv(vertexShader, GL_COMPILE_STATUS, &success);
	if (!success)
	{
		glGetShaderInfoLog(vertexShader, 512, NULL, infoLog);
		std::cout << "ERROR::SHADER::VERTEX::COMPILATION_FAILED\n" << infoLog << std::endl;
	}
	//检测是否编译成功

	unsigned int shaderProgram;//创建着色器程序对象，保存多个着色器合并之后最终链接完成的版本
	shaderProgram = glCreateProgram();

	glAttachShader(shaderProgram, vertexShader);//附加到shaderProgram上
	glAttachShader(shaderProgram, fragmentShader);
	glLinkProgram(shaderProgram);//链接
	
	unsigned int shaderProgram2;
	shaderProgram2 = glCreateProgram();
	glAttachShader(shaderProgram2, vertexShader);
	glAttachShader(shaderProgram2, fragmentShader2);
	glLinkProgram(shaderProgram2);
	//检测是否链接成功
	glGetProgramiv(shaderProgram, GL_LINK_STATUS, &success);
	if (!success)
	{
		glGetProgramInfoLog(shaderProgram2, 512, NULL, infoLog);
		std::cout << "ERROR::SHADERPROGRAM::LINK_FAILED\n" << infoLog << std::endl;
	}
	//检测是否链接成功

	//glUseProgram(shaderProgram);//激活,每个着色器调用和渲染调用都会使用这个程序对象（也就是之前写的着色器)了。
	glDeleteShader(vertexShader);//删除，不再需要这两个shader了
	glDeleteShader(fragmentShader);
	#pragma endregion
    
	float vertices[] = {
		0.5f, 0.5f, 0.0f,   // 右上角
		0.5f, -0.5f, 0.0f,  // 右下角
		0.5f, -0.5f, 0.0f,// 左下角
		0.5f, -0.5f, 0.0f,  // 右下角
		0.5f, -0.5f, 0.0f,// 左下角
		0.5f, 0.5f, 0.0f,   // 右上角

	};
	float vertices2[] = {

		0.5f, 0.5f, 0.0f,   // 右上角
		-0.5f, 0.5f, 0.0f,  // 左上角
		-0.5f, -0.5f, 0.0f, // 左下角
		-0.5f, 0.5f, 0.0f,  // 左上角
		0.5f, 0.5f, 0.0f,   // 右上角
		-0.5f, -0.5f, 0.0f, // 左下角

	};
	unsigned int indices[] = {
		// 注意索引从0开始! 
		// 此例的索引(0,1,2,3)就是顶点数组vertices的下标，
		// 这样可以由下标代表顶点组合成矩形

		0, 1, 3, // 第一个三角形
		1, 2, 3  // 第二个三角形
	};

	#pragma region GenBufferToRender VBO VAO EBO
    unsigned int VBO;//顶点缓冲对象ID,本质上是用以存储glGenBuffers函数创建的缓冲的内存地址
	glGenBuffers(1, &VBO);//创建缓冲对象，此时不知道是什么对象，将其地址赋给VBO，之后访问这个对象是通过VBO访问
	glBindBuffer(GL_ARRAY_BUFFER, VBO);//将VBO对象绑定到GL_ARRAY_BUFFER（顶点缓冲的缓冲类型）上，给定此对象的类型
	glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);
	/*glBufferData是一个专门用来把用户定义的数据复制到当前绑定缓冲的函数。
	第一个参数是目标缓冲的类型：顶点缓冲对象当前绑定到GL_ARRAY_BUFFER目标上
	第二个参数指定传输数据的大小(以字节为单位)；用一个简单的sizeof计算出顶点数据大小就行
	第三个参数是我们希望发送的实际数据。
	第四个参数指定了我们希望显卡如何管理给定的数据。*/
	
	unsigned int VAO;//顶点数组对象
	glGenVertexArrays(1, &VAO);
	// 1. 绑定VAO
	glBindVertexArray(VAO);
	//// 2. 把顶点数组复制到缓冲中供OpenGL使用
	//glBindBuffer(GL_ARRAY_BUFFER, VBO);
	//glBufferData(GL_ARRAY_BUFFER, sizeof(vertices), vertices, GL_STATIC_DRAW);
	// 3. 设置顶点属性指针
	glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
	glEnableVertexAttribArray(0);

	//下面演示的VBO2与VAO2，其实可以用数组代替。如VBOs[0],VBOs[1];VAOs[0],VAOs[1]。
	unsigned int VBO2;
	glGenBuffers(1, &VBO2);
	glBindBuffer(GL_ARRAY_BUFFER, VBO2);
	glBufferData(GL_ARRAY_BUFFER, sizeof(vertices2), vertices2, GL_STATIC_DRAW);

	unsigned int VAO2;
	glGenVertexArrays(1, &VAO2);
	glBindVertexArray(VAO2);
	glVertexAttribPointer(0, 3, GL_FLOAT, GL_FALSE, 3 * sizeof(float), (void*)0);
	glEnableVertexAttribArray(0);

	unsigned int EBO;
	glGenBuffers(1, &EBO);
	glBindBuffer(GL_ELEMENT_ARRAY_BUFFER, EBO);
	glBufferData(GL_ELEMENT_ARRAY_BUFFER, sizeof(indices), indices, GL_STATIC_DRAW);


	glBindBuffer(GL_ARRAY_BUFFER, 0);
	glBindVertexArray(0);
	#pragma endregion

	while (!glfwWindowShouldClose(window))//渲染循环,start to render
	{
		//输入
		processInput(window);
		
		//渲染指令
		glClearColor(0.2f, 0.3f, 0.3f, 1.0f);//状态设置函数
		glClear(GL_COLOR_BUFFER_BIT);//状态使用函数，清空颜色缓冲

		glUseProgram(shaderProgram);
		glBindVertexArray(VAO);
		glDrawArrays(GL_LINES, 0, 6);
		//glDrawElements(GL_TRIANGLES, 6, GL_UNSIGNED_INT, 0);
		glUseProgram(shaderProgram2);
		glBindVertexArray(VAO2);
		glDrawArrays(GL_LINES, 0, 6);
		glBindVertexArray(0);
		//检查并调用事件，交换缓冲
		glfwSwapBuffers(window);
		glfwPollEvents();
	}
	glDeleteVertexArrays(1, &VAO);
	glDeleteBuffers(1, &VBO);
	glDeleteProgram(shaderProgram);
	glfwTerminate();//释放所有资源
	return 0;
}
void framebuffer_size_callback(GLFWwindow* window, int width, int height)
{
	glViewport(0, 0, width, height);//width和height即为窗口的新维度
}
//我们可以对窗口注册一个回调函数(Callback Function)，它会在每次窗口大小被调整的时候被调用。

void processInput(GLFWwindow* window)
{
	if (glfwGetKey(window, GLFW_KEY_ESCAPE) == GLFW_PRESS)//检测按下esc键退出。若没按，glfwGetKey返回GLFW_RELEASE
		glfwSetWindowShouldClose(window, true);
	if (glfwGetKey(window, GLFW_KEY_Q) == GLFW_PRESS)
		glPolygonMode(GL_FRONT_AND_BACK, GL_LINE);
	if (glfwGetKey(window, GLFW_KEY_E) == GLFW_PRESS)
		glPolygonMode(GL_FRONT_AND_BACK, GL_FILL);
}
