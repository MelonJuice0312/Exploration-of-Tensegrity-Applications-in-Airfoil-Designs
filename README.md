import numpy as np # for matrix operations and plotting
from matplotlib import pyplot as plt # for plotting commands

O=([[0,0]])
N = np.array([[2, 1.2, 0, 1.2, 4, 3, 3, 6, 5, 5],[0, 0.75, 0, -0.75, 0, 0.6, -0.6, 0, 0.3, -0.3]]) # node matrix
print(N)
print(O)

C_b = np.array([[0, 1, 0, -1, 0, 0, 0, 0, 0, 0],[1, 0, -1, 0, 0, 0, 0, 0, 0, 0],[0, 0, 0, 0, 0, 1, -1, 0, 0, 0], [-1, 0, 0, 0, 1, 0, 0, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, 0, 1, -1], [0, 0, 0, 0, -1, 0, 0, 1, 0, 0]])

C_s = np.array([[-1, 1, 0, 0, 0, 0, 0, 0, 0, 0],[0,-1, 1, 0, 0, 0, 0, 0, 0, 0],[0, 0, -1, 1, 0, 0, 0, 0, 0, 0],[1, 0, 0, -1, 0, 0, 0, 0, 0, 0], [0, 0, 0, 0, -1, 1, 0, 0, 0, 0], [1, 0, 0, 0, 0, -1, 0, 0, 0, 0], [-1, 0, 0, 0, 0, 0, 1, 0, 0, 0], [0, 0, 0, 0, 1, 0, -1, 0, 0, 0], [0, 0, 0, 0, 0, 0, 0, -1, 1, 0], [0, 0, 0, 0, 1, 0, 0, 0, -1, 0], [0, 0, 0, 0, 1, 0, 0, 0, -1, 0], [0, 0, 0, 0, -1, 0, 0, 0, 0, 1], [0, 0, 0, 0, 0, 0, 0, 1, 0, -1]])

print(C_b)

# plotting function

def tenseg_plot(N,C_b,C_s):
   
    # compute the bar and string matrices
    B = N @ (C_b.transpose()) # bar matrix
    S = N @ (C_s.transpose()) # string matrix

    # print statements
    print("B =", B)
    print("S =", S)
   
    # plot the bar vectors
    bar_start_nodes = np.zeros((2, B.shape[1])).astype(float)
    bar_end_nodes = np.zeros((2, B.shape[1])).astype(float)
    for j in range(B.shape[1]):
        bar_start_ind = C_b[j,:]==-1
        bar_end_ind = C_b[j,:]==1
        bar_start_nodes[:,j] = N[:,np.where(bar_start_ind)[0]].transpose()
        bar_end_nodes[:,j] = N[:,np.where(bar_end_ind)[0]].transpose()
   
    bar_lengths = np.linalg.norm(B,axis=0)
    plt.quiver(bar_start_nodes[0,:],bar_start_nodes[1,:],B[0,:],B[1,:],angles='xy',scale_units='xy', scale=1,headaxislength=0,headlength=0,headwidth=0,color='k')
   
    # plot the string vectors
    string_start_nodes = np.zeros((2, S.shape[1])).astype(float)
    string_end_nodes = np.zeros((2, S.shape[1])).astype(float)
    for j in range(S.shape[1]):
        string_start_ind = C_s[j,:]==-1
        string_end_ind = C_s[j,:]==1
        string_start_nodes[:,j] = N[:,np.where(string_start_ind)[0]].transpose()
        string_end_nodes[:,j] = N[:,np.where(string_end_ind)[0]].transpose()
   
    string_lengths = np.linalg.norm(S,axis=0)
    plt.quiver(string_start_nodes[0,:],string_start_nodes[1,:],S[0,:],S[1,:],angles='xy',scale_units='xy', scale=1,headaxislength=0,headlength=0,headwidth=0,color='r')
   
    # setting the plot parameters
    plt.xlim([-2, 2])
    plt.ylim([-2, 2])
    plt.xlabel('x')
    plt.ylabel('y')
    plt.axis('equal')
    plt.axis('equal')
    plt.grid(which='minor', color='#DDDDDD', linestyle=':', linewidth=0.5)
    plt.minorticks_on()
    plt.show()
   
    return plt

#plot
fig_handle = tenseg_plot(N,C_b,C_s)
