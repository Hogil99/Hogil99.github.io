경사 하강법

```
import torch
import torch.nn.functional as F

# create the target tensor
target = torch.FloatTensor([[.1, .2, .3],
                           [.4, .5, .6],
                           [.7, .8, .9]])

# create a random tensor with the same shape as the target tensor
x = torch.rand_like(target)

# set the requires_grad attribute of x to True, so that gradients will be calculated for x
x.requires_grad = True

# print the initial value of x
print(x)

# calculate the mean squared error loss between x and the target tensor
loss = F.mse_loss(x, target)
loss

# set the threshold for the loss
threshold = 1e-5

# set the learning rate for the optimization algorithm
learning_rate = 1.

# Initialize iteration counter
iter_cnt = 0

# loop until the loss is less than the threshold
while loss > threshold:
    iter_cnt += 1
    # calculate the gradients of the loss with respect to x
    loss.backward()
    
    # update the values of x by subtracting the gradients scaled by the learning rate
    x = x - learning_rate * x.grad
    
    # disconnect the tensor from the computation graph and set the requires_grad attribute of x to True again
    x.detach_()
    x.requires_grad_(True)
    
    # calculate the new loss after the update
    loss = F.mse_loss(x, target)
    
    # print the current loss and values of x
