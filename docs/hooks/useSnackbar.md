# useSnackbar (MUI)

To display and control MUI snackbars more easily we can create an hook.

```jsx
import { Alert as MuiAlert, Snackbar } from "@mui/material";
import {
  createContext,
  ReactNode,
  useContext,
  useEffect,
  useState,
} from "react";

const AUTO_DISMISS = 5000;

type Alert = {
  message: string;
  severity?: "success" | "info" | "warning" | "error";
};

const SnackBarContext = createContext({ setAlert: (alert: Alert) => {} });

export const SnackBarProvider = ({ children }: { children: ReactNode }) => {
  const [alerts, setAlerts] = useState<Alert[]>([]);

  const activeAlertIds = alerts.join(",");

  // Used to dismiss the alert if the user don't do it
  useEffect(() => {
    if (activeAlertIds.length > 0) {
      const timer = setTimeout(
        () => setAlerts((alerts) => alerts.slice(0, alerts.length - 1)),
        AUTO_DISMISS
      );
      return () => clearTimeout(timer);
    }
  }, [activeAlertIds]);

  const setAlert = (alert: Alert) => {
    const nextAlerts = [alert, ...alerts];
    setAlerts(nextAlerts);
  };

  return (
    <SnackBarContext.Provider value={{ setAlert }}>
      {children}
      {alerts.map((alert) => (
        <Snackbar
          open={true}
          anchorOrigin={{
            vertical: "bottom",
            horizontal: "left",
          }}
          key={alert.message}
          autoHideDuration={5000}
        >
          <MuiAlert severity={alert.severity} sx={{ width: "100%" }}>
            {alert.message}
          </MuiAlert>
        </Snackbar>
      ))}
    </SnackBarContext.Provider>
  );
};

export const useSnackBars = () => useContext(SnackBarContext);
```

## Usage

```jsx
const App = () => (
  <SnackBarProvider>
    <Dummy />
  </SnackBarProvider>
);

const Dummy = () => {
  const { setAlert } = useSnackbar();

  return (
    <button
      onClick={() =>
        setAlert({
          message: "Testing",
          severity: "info",
        })
      }
    >
      Test
    </button>
  );
};
```
