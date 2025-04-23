def compare_methods(f, df, a, b, x0, tol=1e-6, max_iter=100):
    # Bisection Method
    def bisection(f, a, b, tol, max_iter):
        steps = 0
        if f(a) * f(b) >= 0:
            raise ValueError("Function has same signs at a and b.")
        while (b - a) / 2 > tol and steps < max_iter:
            c = (a + b) / 2
            if f(c) == 0:
                break
            elif f(a) * f(c) < 0:
                b = c
            else:
                a = c
            steps += 1
        return c, steps

    # Newton-Raphson Method
    def newton_raphson(f, df, x0, tol, max_iter):
        steps = 0
        x = x0
        while abs(f(x)) > tol and steps < max_iter:
            dfx = df(x)
            if dfx == 0:
                raise ValueError("Zero derivative encountered.")
            x -= f(x) / dfx
            steps += 1
        return x, steps

    try:
        root_bisect, steps_bisect = bisection(f, a, b, tol, max_iter)
    except Exception as e:
        root_bisect, steps_bisect = None, str(e)

    try:
        root_newton, steps_newton = newton_raphson(f, df, x0, tol, max_iter)
    except Exception as e:
        root_newton, steps_newton = None, str(e)

    return {
        "Bisection": {"root": root_bisect, "steps": steps_bisect},
        "Newton-Raphson": {"root": root_newton, "steps": steps_newton}
    }
    
