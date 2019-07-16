Matplotlib
============

loglogplot
-----------------


.. code-block:: python
    :linenos:

    def loglogplot(x,y,labelx="LogPop",labely="LogArea"):
        plt.style.use('ggplot')
        xd,yd=np.log10(x),np.log10(y)
        # make the scatter plot
        fig, ax = plt.subplots(figsize=(5,4))

        ax.set_yscale("log")
        ax.set_xscale("log")
        #ax.set_ylim(1e1,10**3.4)
        #ax.set_xlim(10**(-0.1),10**(2.5))
        # determine best fit line
        par = np.polyfit(xd, yd, 1,full=True)

        slope=par[0][0]
        intercept=par[0][1]

        xl = [min(xd), max(xd)]
        yl = [slope*xx + intercept  for xx in xl]
        ax.plot([10**xx for xx in xl],[10**yy for yy in yl], '-m')

        # coefficient of determination, plot text
        variance = np.var(yd)#方差
        residuals = np.var([(slope*xx + intercept - yy)  for xx,yy in zip(xd,yd)])#残差

        Rsqr = np.round(1-residuals/variance, decimals=2)
        #ax.text(0.3*max(x),1.2*max(y),r'$R^2 ={0.2f} \n Slope={0.2f}'.format{Rsqr,slope.round(2)}, fontsize=15)
        ax.text(min(x),1.1*max(y),'%s =%0.2f\nSlope=%0.2f\nIntercept=%0.2f'%("$\sf{R^2}$",Rsqr,slope.round(2),intercept), fontsize=8)
        print(variance,residuals)
        plt.xlabel(labelx)
        plt.ylabel(labely)

        # error bounds
        yerr = [abs(slope*xx + intercept - yy)  for xx,yy in zip(xd,yd)]
        par = np.polyfit(xd, yerr, 2, full=True)
        erro_x2 = par[0][0]
        erro_x1 = par[0][1]
        erro_x0 = par[0][2]
        yerrUpper = [(xx*slope+intercept)+(erro_x2*xx**2 + erro_x1*xx + erro_x0) for xx,yy in zip(xd,yd)]
        yerrLower = [(xx*slope+intercept)-(erro_x2*xx**2 + erro_x1*xx + erro_x0) for xx,yy in zip(xd,yd)]
        print(erro_x2, erro_x1, erro_x0)

        #     ax.plot([10**xx for xx in xd],[10**yy for yy in yerrLower], 'm')
        #     ax.plot([10**xx for xx in xd],[10**yy for yy in yerrUpper], 'm')
        ax.scatter(x, y, s=10, alpha=1, marker='h',color="orangered")

        yline = [slope*xl[0] + intercept,xl[1] + intercept]
        #yline2 = [xl[0] + intercept,slope*xl[1] + intercept]
        ax.plot([10**xx for xx in xl],[10**yy for yy in yline], '--m')
        #ax.plot([10**xx for xx in xl],[10**yy for yy in yline2], '--m')
        ax.text(250,660, "Slope=1", size = 5,\
                family = "fantasy", color = "r", style = "italic", weight = "light",\
                )#bbox = dict(facecolor = "r", alpha = 0.2)
        return slope,ax

.. image:: /img/loglogplot.png
